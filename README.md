# Project Allocation Platform

A full-stack, automated web application built for universities to streamline the entire capstone project allocation lifecycle. It removes manual bias, ensures fair distribution, and provides complete transparency through a structured, multi-phase workflow involving four distinct user roles: **Admins**, **Subadmins**, **Faculty**, and **Students**.

The platform handles the creation of project pools, faculty proposal submissions, multi-level review and approval processes, student team formation, and conflict-free project selection.

---

## 🌟 Key Features

### Role-Based Access
1. **Admin (`ADMIN`)**
   - Create and manage academic Pools (Year/Semester).
   - Bulk import Users (Students, Faculty, Subadmins) via CSV.
   - Advance the phases of the allocation pool strictly in order.
   - Make final decisions (Approve/Reject) on projects marked as "Held" by Subadmins.
   - View global statistics and export reports.
2. **Subadmin (`SUBADMIN`)**
   - Review 4 proposals submitted by an assigned Faculty member.
   - **Mandatory Review Limit:** Must "Lock" (Approve) 3 proposals and put 1 on "Hold" for the Admin's review.
3. **Faculty (`FACULTY`)**
   - Submit exactly 4 project proposals during the "Submission Open" phase.
   - Edit draft proposals.
   - Track their proposals' review status (Locked, Held, Approved, Rejected).
   - View the Final Team assigned to their approved projects.
4. **Student (`STUDENT`)**
   - View approved projects once the "Selection Open" phase begins.
   - Create teams and invite peers from the same pool.
   - Send and Accept/Decline team invites.
   - **Conflict-Free Selection:** Form a team and select an available project. Project locking is handled via atomic database transactions to prevent race conditions.
   - **Submit Custom Ideas:** Pitch custom projects; if approved, the project is reserved exclusively for the student's team.

### Primary Workflow Lifecycles
The allocation process follows a strict phased approach controlled by the Admin:
`DRAFT` → `SUBMISSION_OPEN` → `UNDER_REVIEW` → `DECISION_PENDING` → `SELECTION_OPEN` → `TEAMS_FORMING` → `FROZEN`

---

## 🛠️ Technology Stack

### Frontend (Client-Side)
- **Framework:** React + TypeScript
- **Build Tool:** Vite
- **Styling:** Tailwind CSS
- **State Management:** Zustand
- **Routing:** React Router v6
- **Data Fetching:** Axios (with interceptors for JWT token refresh)
- **Icons & UI:** Lucide React, React Hot Toast

### Backend (Server-Side)
- **Runtime:** Node.js / Express.js (TypeScript)
- **Database ORM:** Prisma ORM
- **Database Engine:** PostgreSQL
- **Authentication:** JSON Web Tokens (JWT) for Access/Refresh tokens + bcrypt
- **File Parsing (CSV):** Multer & `csv-parser`
- **Security:** Helmet, CORS, `express-rate-limit`

---

## 🚀 Getting Started

Follow these steps to get the project up and running on your local machine.

### Prerequisites
- [Node.js](https://nodejs.org/) (v16.x or newer recommended)
- [PostgreSQL](https://www.postgresql.org/) (ensure it is running locally)

### 1. Clone the repository

```bash
git clone https://github.com/YourUsername/project-allocation-platform.git
cd project-allocation-platform
```

### 2. Backend Setup
Navigate to the `backend` directory, install dependencies, set up the environment, and run database migrations.

```bash
cd backend

# Install dependencies
npm install

# Create a .env file and update the variables (especially DATABASE_URL)
cp .env.example .env

# Run Prisma schema push & seed the database with the default Admin user
npx prisma db push
npx prisma generate
npm run seed

# Run the backend server in development mode
npm run dev
```

### 3. Frontend Setup
Open a new terminal, navigate to the `frontend` directory, install dependencies, and start the development server.

```bash
cd frontend

# Install dependencies
npm install

# Create a .env file and set the backend API URL
cp .env.example .env

# Run the frontend server in development mode
npm run dev
```

The frontend will be accessible at `http://localhost:5173` and the backend strictly via `http://localhost:5000`.

### Default Admin Credentials
When you run the seed script (`npm run seed`), a default Admin account is created:
- **Email:** `admin@iul.ac.in`
- **Password:** `Admin@123456`

---

## 📂 Folder Structure

```text
project-allocation-platform/
├── backend/
│   ├── prisma/                # Database schema and seed script
│   └── src/
│       ├── config/            # Environment configurations
│       ├── middleware/        # Express security & validation middlewares
│       ├── modules/           # Features separated by domain (users, pools, projects, teams)
│       └── shared/            # Utilities (JWT handling, pagination, error formatting)
└── frontend/
    └── src/
        ├── components/        # Reusable UI elements (Layouts, Inputs, Buttons)
        ├── config/            # Frontend configuration (Axios instances)
        ├── pages/             # Pages separated by Roles (Admin, Faculty, Subadmin, Student)
        ├── services/          # API SDK wrapper functions
        ├── stores/            # Zustand global stores
        └── types/             # Shared TypeScript types and interfaces
```
