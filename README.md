<div align="center">

<img src="logo.png" width="150" alt="Lumière French Academy Logo"/>

# 🇫🇷 Lumière French Academy

### A Full-Stack French Language Learning & Academy Management Platform

<p>
  <b>React • TypeScript • FastAPI • PostgreSQL • JWT • RBAC • REST API • Tailwind CSS • Docker</b>
</p>

> A production-oriented platform connecting a premium French-learning website with authentication, enrollment workflows, batch operations, student portals, administration, analytics, and a PostgreSQL-backed REST API.

</div>

---

## ✨ What is Lumière?

**Lumière French Academy** is more than a marketing website. It is a complete digital platform designed around the real workflow of a language academy.

A learner can discover a course, submit an enrollment request, create an account, access a protected dashboard, view their batch and schedule, and join their live class. Administrators can manage courses, review enrollment requests, activate students, create batches, assign learners, manage contact requests, update website content, and view operational analytics.

The platform is built around four layers:

- 🌐 **Public academy experience**
- 🎓 **Student portal**
- 🛡️ **Teacher/Admin operations**
- ⚡ **FastAPI + PostgreSQL application backend**

---

# 🚀 Core Capabilities

| Area | Capabilities |
|---|---|
| 🌐 Public Website | Landing page, courses, why us, teacher profile, testimonials, FAQs, contact |
| 🔐 Authentication | Registration, login, JWT authentication, protected routes |
| 🧑‍🎓 Student Portal | Overview, course, batch, schedule, teacher, profile, fee status |
| 📝 Enrollment | Enrollment requests, approval, rejection, activation, batch assignment |
| 👨‍🏫 Batch Management | Create, edit, delete, schedule, capacity, teacher, meeting link, student assignment |
| 📚 Course Management | Create, edit, delete and manage academy courses |
| 👥 Student Management | Student listing, details, activation, suspension and deletion |
| 📞 Contact Management | Website enquiries, contact records and admin handling |
| 📊 Analytics | Enrollment growth, admission funnel, active students, revenue and courses |
| 🧩 Website CMS | API-driven website content with protected admin editing |
| 🔌 REST API | Swagger/OpenAPI documented backend with protected administrative endpoints |

---

# 🏗️ Architecture

```text
┌──────────────────────────────────────────────────────────────┐
│                    Lumière Web Application                  │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  Public Website                    Protected Application      │
│  ───────────────                   ─────────────────────     │
│  Home                             Student Portal             │
│  Courses                          Teacher/Admin Portal        │
│  Why Us                           Analytics                   │
│  Teacher                          Batch Management            │
│  Testimonials                     Course Management           │
│  FAQs                             Enrollment Management       │
│  Contact                          Student Management          │
│                                                              │
└──────────────────────────────┬───────────────────────────────┘
                               │
                               │ HTTPS / JSON REST
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                        FastAPI Backend                       │
├──────────────────────────────────────────────────────────────┤
│  Authentication / Authorization                              │
│  ├── Register / Login                                        │
│  ├── JWT validation                                          │
│  └── Role-based access control                               │
│                                                              │
│  Domain Modules                                               │
│  ├── Users                                                    │
│  ├── Courses                                                  │
│  ├── Enrollments                                              │
│  ├── Batches                                                  │
│  ├── Students                                                 │
│  ├── Contacts                                                 │
│  └── Website Content                                          │
│                                                              │
│  Validation + Serialization                                  │
│  └── Pydantic request/response schemas                        │
└──────────────────────────────┬───────────────────────────────┘
                               │
                               │ SQL / ORM layer
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                         PostgreSQL                            │
├──────────────────────────────────────────────────────────────┤
│  users • courses • enrollments • batches • contacts         │
│  testimonials • website_content                              │
└──────────────────────────────────────────────────────────────┘
```

---

# 🔐 Authentication & RBAC

Lumière uses **role-aware access control** so the public website, learner experience, and administrative operations have different access boundaries.

### 👤 Student

Students can access their own learner experience:

- View personal dashboard
- View enrolled course
- View assigned batch
- View teacher and timetable
- Access the live class link
- Check fee status
- Manage profile information
- Contact the academy

### 🧑‍💼 Teacher / Admin

Administrative users can access operational tools:

- Course CRUD
- Enrollment review
- Approve / reject requests
- Activate learners
- Assign students to batches
- Create and manage batches
- Manage students
- Manage contact requests
- Update website content
- View analytics

### 🔒 API-level authorization

Sensitive actions are protected at the backend layer rather than relying only on frontend visibility. This keeps administrative operations behind authenticated and authorized API routes.

---

# 🔄 Enrollment Lifecycle

```text
Visitor
   │
   ▼
Explore Course
   │
   ▼
Submit Enrollment Request
   │
   ▼
Admin Review
   ├───────────────┐
   │               │
Reject           Approve
   │               │
   ▼               ▼
Closed       Activate / Assign
                  │
                  ▼
             Student Portal
                  │
                  ▼
             Batch Assignment
                  │
                  ▼
          Schedule + Teacher
                  │
                  ▼
             Join Live Class
```

This models an actual academy admission workflow instead of treating enrollment as a standalone form.

---

# 📡 API Overview

The FastAPI backend exposes an OpenAPI/Swagger-documented REST surface.

## Authentication

```http
POST /auth/register
POST /auth/login
```

## Courses

```http
GET  /courses
POST /courses
GET  /courses/{slug}
```

## Current User

```http
GET /users/me
```

## Enrollments

```http
POST  /enrollments
GET   /enrollments/me
GET   /enrollments/{enrollment_id}
GET   /enrollments/admin/all

PATCH /enrollments/admin/{enrollment_id}/approve
PATCH /enrollments/admin/{enrollment_id}/reject
PATCH /enrollments/admin/{enrollment_id}/activate
PATCH /enrollments/admin/{enrollment_id}/assign-batch
```

## Batches

```http
GET    /batches
GET    /batches/me
GET    /batches/{batch_id}
POST   /batches/admin
PUT    /batches/admin/{batch_id}
DELETE /batches/admin/{batch_id}
```

## Students

```http
GET    /admin/students
GET    /admin/students/{student_id}
DELETE /admin/students/{student_id}
PATCH  /admin/students/{student_id}/activate
PATCH  /admin/students/{student_id}/suspend
```

## Contact

```http
POST   /contact
GET    /admin/contact
GET    /admin/contact/{contact_id}
PATCH  /admin/contact/{contact_id}
DELETE /admin/contact/{contact_id}
```

## Website Content

```http
GET /website-content
PUT /admin/website-content
```

---

# 🗄️ PostgreSQL Database

The platform uses **PostgreSQL** for persistent relational application data.

### Core entities

| Table | Responsibility |
|---|---|
| `users` | Accounts, identity and authentication data |
| `courses` | Course catalogue and course metadata |
| `enrollments` | Admission and enrollment lifecycle |
| `batches` | Class schedule, teacher, capacity and meeting information |
| `contact_requests` | Website leads and enquiries |
| `testimonials` | Learner/parent testimonials |
| `website_content` | Dynamic CMS-style website content |

### Relationship overview

```text
users
  │
  └── enrollments ─── courses
          │
          └────────── batches

contact_requests
testimonials
website_content
```

The relational model allows the frontend dashboards and public website to consume live application data instead of depending on static page content.

---

# 🧰 Technology Stack

## Frontend

- ⚛️ React
- 🟦 TypeScript
- ⚡ Vite
- 🎨 Tailwind CSS
- 🧩 shadcn/ui
- 📊 Analytics visualizations
- 🔐 Protected application routes

## Backend

- 🐍 Python
- ⚡ FastAPI
- ✅ Pydantic
- 🔐 JWT authentication
- 🛡️ Role-based authorization
- 📡 REST APIs
- 📘 OpenAPI / Swagger

## Database

- 🐘 PostgreSQL

## Engineering

- 🐳 Docker-ready setup
- 🔧 Git
- 🐙 GitHub
- 🔌 REST-based frontend/backend integration

---

# 📸 Application Showcase

## 🏠 Landing Page

### Light Mode

<p align="center">
  <img src="Landing_Page_White.png" width="92%" alt="Lumière landing page light mode"/>
</p>

### Dark Mode

<p align="center">
  <img src="Landing_Page_Dark.png" width="92%" alt="Lumière landing page dark mode"/>
</p>

---

## 📚 Courses

<p align="center">
  <img src="Courses_Page.png" width="92%" alt="Lumière courses page"/>
</p>

The catalogue presents learning paths across **School French, DELF Prim, DELF Junior and Adult DELF**, with course level, class frequency and learner-focused descriptions.

---

## 💡 Why Lumière

<p align="center">
  <img src="Why_Us_Page.png" width="92%" alt="Why Lumière page"/>
</p>

---

## 👩‍🏫 Teacher Profile

<p align="center">
  <img src="Teacher_Page.png" width="92%" alt="Lumière teacher page"/>
</p>

---

## ⭐ Testimonials

<p align="center">
  <img src="Testimonals_Page.png" width="92%" alt="Lumière testimonials page"/>
</p>

---

## ❓ FAQs

<p align="center">
  <img src="FAQs_Page.png" width="92%" alt="Lumière FAQs page"/>
</p>

---

## 📞 Contact & Demo Booking

<p align="center">
  <img src="Contact_Page.png" width="92%" alt="Lumière contact page"/>
</p>

Website enquiries are captured into the backend and can subsequently be handled from the admin console.

---

# 🎓 Student Portal

## Student Dashboard

<p align="center">
  <img src="Student_Dashboard.png" width="92%" alt="Lumière student dashboard"/>
</p>

The student experience brings together:

- Current course
- Assigned batch
- Schedule
- Teacher
- Enrollment state
- Quick access to batch details
- Academy contact

## My Batch

<p align="center">
  <img src="Student_MyBatch.png" width="92%" alt="Lumière student batch page"/>
</p>

Students can view:

- Class days
- Class time
- Teacher
- Batch strength
- Start and end dates
- Live class entry point

---

# 🛡️ Admin / Teacher Console

The administration area is designed as the operational control center for the academy.

## Admin Dashboard

<p align="center">
  <img src="Admin_Dashboard.png" width="92%" alt="Lumière admin dashboard"/>
</p>

It surfaces operational metrics including:

- Active students
- Pending requests
- Batches
- Monthly revenue
- New enquiries
- Awaiting payment

## Enrollment Requests

<p align="center">
  <img src="Admin_Enrollments.png" width="92%" alt="Lumière admin enrollment requests"/>
</p>

Admins can review incoming requests and progress them through the admission lifecycle.

## Course Management

<p align="center">
  <img src="Admin_CourseManagement.png" width="92%" alt="Lumière admin course management"/>
</p>

## Batch Management

<p align="center">
  <img src="Admin_Batch.png" width="92%" alt="Lumière admin batch management"/>
</p>

## Create Batch

<p align="center">
  <img src="Admin_CreateBatch.png" width="92%" alt="Lumière create batch form"/>
</p>

Batch creation supports course, code, name, teacher, capacity, days, time, start/end dates and meeting-link details.

## Analytics

<p align="center">
  <img src="Admin_Analytics.png" width="92%" alt="Lumière admin analytics"/>
</p>

The analytics workspace presents:

- Enrollment growth
- Admission funnel
- Active students
- Monthly revenue
- Courses live

## Contact Requests

<p align="center">
  <img src="Admin_Contact.png" width="92%" alt="Lumière admin contact requests"/>
</p>

---

# 📘 API Documentation

<p align="center">
  <img src="api.png" width="92%" alt="Lumière FastAPI Swagger API documentation"/>
</p>

The API documentation exposes the application contract for:

- Authentication
- Users
- Courses
- Enrollments
- Batches
- Students
- Contact requests
- Website content

It also exposes structured request/response schemas through OpenAPI.

---

# 🗄️ Database View

<p align="center">
  <img src="PostgreSQL_Db.png" width="92%" alt="Lumière PostgreSQL database"/>
</p>

The database layer is the persistent foundation for the academy's user, course, enrollment, batch, contact, testimonial and website-content workflows.

---

# 🔐 Security Model

```text
HTTP Request
     │
     ▼
JWT Authentication
     │
     ▼
Current User
     │
     ▼
Role / Permission Check
     │
     ├── Student → own learner resources
     │
     └── Admin  → management resources
     │
     ▼
Business Logic
     │
     ▼
PostgreSQL
```

Security-oriented capabilities include:

- Password hashing
- JWT-based authentication
- Protected API routes
- Role-aware authorization
- Pydantic validation
- Backend-side permission checks
- Separation of public and administrative operations

---

# 📈 Product Workflow

```text
                         ┌───────────────┐
                         │  Public Site  │
                         └───────┬───────┘
                                 │
                  ┌──────────────┼──────────────┐
                  ▼              ▼              ▼
               Courses        Contact        Register
                  │              │              │
                  ▼              ▼              ▼
             Enrollment       Enquiry          Login
                  │                             │
                  └──────────────┬──────────────┘
                                 ▼
                         ┌─────────────────┐
                         │  Admin Review   │
                         └────────┬────────┘
                                  │
                         Approve / Reject
                                  │
                                  ▼
                         ┌─────────────────┐
                         │ Assign a Batch  │
                         └────────┬────────┘
                                  │
                                  ▼
                         ┌─────────────────┐
                         │ Student Portal  │
                         └────────┬────────┘
                                  │
                                  ▼
                     Schedule / Teacher / Class
```

---

# ✅ Feature Matrix

| Module | Status |
|---|:---:|
| Public Website | ✅ |
| Responsive UI | ✅ |
| Light / Dark Theme | ✅ |
| Authentication | ✅ |
| JWT Protected APIs | ✅ |
| RBAC | ✅ |
| Course Management | ✅ |
| Enrollment Workflow | ✅ |
| Batch Management | ✅ |
| Student Management | ✅ |
| Contact Management | ✅ |
| Student Dashboard | ✅ |
| Admin Dashboard | ✅ |
| Analytics Dashboard | ✅ |
| PostgreSQL Persistence | ✅ |
| Swagger / OpenAPI | ✅ |
| Website Content Management | ✅ |
| Docker-ready Setup | ✅ |

---

# 📁 Repository Showcase Assets

```text
.
├── Landing_Page_Dark.png
├── Landing_Page_White.png
├── Courses_Page.png
├── Why_Us_Page.png
├── Teacher_Page.png
├── Testimonals_Page.png
├── FAQs_Page.png
├── Contact_Page.png
│
├── Student_Dashboard.png
├── Student_MyBatch.png
│
├── Admin_Dashboard.png
├── Admin_Enrollments.png
├── Admin_CourseManagement.png
├── Admin_Batch.png
├── Admin_CreateBatch.png
├── Admin_Analytics.png
├── Admin_Contact.png
│
├── PostgreSQL_Db.png
├── api.png
├── register.png
├── logo.png
└── README.md
```

---

# 🎯 Engineering Philosophy

> **Keep the learner experience elegant while keeping the operational system structured.**

The project emphasizes:

- 🧱 Clear separation of responsibilities
- 🔐 Secure access boundaries
- 🗄️ Data-driven workflows
- 🔄 Explicit business state transitions
- 🧩 Reusable UI components
- 📡 Clean REST contracts
- 📈 Analytics-ready data
- 🚀 Production-oriented engineering

---

# 🔮 Future Enhancements

Natural next steps for the platform include:

- 💳 Online payment gateway integration
- 📧 Automated email / WhatsApp notifications
- 📅 Attendance tracking
- 📊 Student progress analytics
- 🧑‍🏫 Dedicated teacher dashboard
- 🧾 Invoices and receipts
- 🔔 Real-time notifications
- 🧪 Automated API / integration tests
- ☁️ Production cloud deployment
- 📦 CI/CD pipelines
- 🔎 Advanced reporting and exports

---

# 🔒 Repository Note

This repository is maintained as a **public portfolio and product showcase**.

The README documents the product architecture, API surface, workflows, data model and visual implementation. Application implementation details that are not intended for public distribution are intentionally not reproduced here.

---

<div align="center">

### 🇫🇷 Merci beaucoup!

**Lumière French Academy — Learn French. Build confidence.**

⭐ Star the repository if you enjoyed exploring the project.

</div>
