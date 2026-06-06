## 📋 Todo App

---

### 🎯 Overview
A full-stack task management app where users can register, log in, and manage personal todos with categories, priorities, and due dates.

---

### 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React + Vite, Tailwind CSS |
| Backend | Node.js + Express |
| Database | PostgreSQL |
| Auth | JWT + bcrypt |
| Deployment | Vercel (frontend), Railway (backend + DB) |

---

### 👤 User Stories

*Auth*
- As a user, I can register with email & password
- As a user, I can log in and receive a JWT token
- As a user, I can log out

*Todos*
- As a user, I can create a todo with title, description, due date, priority, and category
- As a user, I can view all my todos
- As a user, I can mark a todo as complete/incomplete
- As a user, I can edit a todo
- As a user, I can delete a todo
- As a user, I can filter todos by status, priority, or category
- As a user, I can search todos by title

---

### 🗄️ Database Schema

*users*

id          UUID PRIMARY KEY
email       VARCHAR UNIQUE NOT NULL
password    VARCHAR NOT NULL
name        VARCHAR
created_at  TIMESTAMP


*todos*

id            UUID PRIMARY KEY
user_id       UUID REFERENCES users(id)
title         VARCHAR NOT NULL
description   TEXT
is_complete   BOOLEAN DEFAULT false
priority      ENUM ('low', 'medium', 'high')
due_date      DATE
category      VARCHAR
created_at    TIMESTAMP
updated_at    TIMESTAMP


---

### 🔌 API Endpoints

*Auth Routes*

POST   /api/auth/register     → Register user
POST   /api/auth/login        → Login, returns JWT
POST   /api/auth/logout       → Invalidate session
GET    /api/auth/me           → Get current user


*Todo Routes* (all protected by JWT middleware)

GET    /api/todos             → Get all todos (with filters)
POST   /api/todos             → Create a todo
GET    /api/todos/:id         → Get single todo
PUT    /api/todos/:id         → Update todo
DELETE /api/todos/:id         → Delete todo
PATCH  /api/todos/:id/toggle  → Toggle complete status


*Query Params for GET /api/todos*

?status=complete|incomplete
?priority=low|medium|high
?category=work|personal|...
?search=keyword
?sortBy=due_date|created_at|priority


---

### 🖥️ Frontend Pages & Components

*Pages*
- / — Landing/marketing page
- /login — Login form
- /register — Register form
- /dashboard — Main todo view

*Key Components*

<Navbar />              → Logo, user info, logout
<TodoList />            → Renders list of TodoCard
<TodoCard />            → Shows title, priority badge, due date, toggle, edit/delete
<TodoForm />            → Create/edit modal with all fields
<FilterBar />           → Filter by status, priority, category
<SearchBar />           → Live search input
<EmptyState />          → Shown when no todos match
<PriorityBadge />       → Color-coded low/medium/high


---

### 🔐 Auth Flow


Register → hash password (bcrypt) → save user → return JWT
Login    → verify password        → return JWT
Request  → JWT in Authorization header (Bearer token)
         → middleware validates JWT → attaches user to req


---

### ✅ Validation Rules

| Field | Rule |
|---|---|
| Email | Valid format, unique |
| Password | Min 8 chars |
| Title | Required, max 100 chars |
| Due Date | Must be today or future |
| Priority | Must be low / medium / high |

---

### 🚀 Feature Roadmap

*Phase 1 — MVP*
- Auth (register/login)
- Basic CRUD for todos
- Mark complete/incomplete

*Phase 2 — Enhanced*
- Filtering & search
- Priority & categories
- Due date with overdue highlighting

*Phase 3 — Polish*
- Drag-and-drop reordering
- Dark mode
- Email reminders for due todos
- Stats dashboard (completed vs pending chart)

---

### 📁 Folder Structure


/client
  /src
    /components
    /pages
    /hooks
    /api        ← axios instance + API calls
    /context    ← AuthContext
/server
  /routes
  /controllers
  /middleware
  /models
  /db          ← DB connection + migrations


---