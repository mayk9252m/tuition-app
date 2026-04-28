# TuitionPro — Tuition Management SaaS

A production-ready, full-stack SaaS application for tuition teachers to manage students, attendance, fees, and analytics.

---

## 🗂 Project Structure

```
tuition-saas/
├── backend/                   # Node.js + Express + MongoDB API
│   ├── controllers/           # Route handlers
│   │   ├── authController.js
│   │   ├── studentsController.js
│   │   ├── attendanceController.js
│   │   ├── feesController.js
│   │   └── analyticsController.js
│   ├── middleware/
│   │   └── auth.js            # JWT protection middleware
│   ├── models/
│   │   ├── User.js            # User schema (bcrypt hashed passwords)
│   │   └── Student.js         # Student, Attendance, FeesHistory schemas
│   ├── routes/
│   │   ├── auth.js
│   │   ├── students.js
│   │   ├── attendance.js
│   │   ├── fees.js
│   │   └── analytics.js
│   ├── server.js              # Express app + MongoDB + Cron job
│   ├── render.yaml            # Render.com deployment config
│   └── .env.example
│
└── frontend/                  # React + Tailwind CSS
    ├── src/
    │   ├── components/
    │   │   ├── layout/
    │   │   │   ├── Sidebar.jsx       # Navigation sidebar
    │   │   │   └── Layout.jsx        # App layout wrapper
    │   │   └── ui/
    │   │       ├── index.jsx         # StatCard, Modal, Badge, Spinner...
    │   │       └── AddStudentModal.jsx
    │   ├── context/
    │   │   └── AuthContext.jsx       # Auth state management
    │   ├── pages/
    │   │   ├── Login.jsx
    │   │   ├── Register.jsx
    │   │   ├── Dashboard.jsx         # Stats overview
    │   │   ├── Students.jsx          # Student management
    │   │   ├── Attendance.jsx        # Daily attendance
    │   │   ├── Fees.jsx              # Fee tracking
    │   │   └── Analytics.jsx         # Recharts dashboard
    │   ├── services/
    │   │   └── api.js                # Axios API client
    │   ├── App.jsx                   # Routes + Auth guards
    │   └── index.css                 # Tailwind + custom styles
    └── vercel.json
```

---

## 🚀 Local Development Setup

### 1. Backend

```bash
cd backend
npm install
cp .env.example .env
# Edit .env with your MongoDB URI, JWT secret, Twilio credentials
npm run dev        # starts on http://localhost:5000
```

### 2. Frontend

```bash
cd frontend
npm install
cp .env.example .env
# Set REACT_APP_API_URL=http://localhost:5000/api
npm start          # starts on http://localhost:3000
```

---

## ☁️ Deployment

### MongoDB Atlas
1. Create a free cluster at [mongodb.com/atlas](https://mongodb.com/atlas)
2. Create a database user with password
3. Whitelist `0.0.0.0/0` (all IPs) for Render/Vercel access
4. Copy the connection string → use as `MONGODB_URI`

### Backend → Render.com
1. Push backend folder to GitHub
2. Go to [render.com](https://render.com) → New Web Service
3. Connect your GitHub repo
4. Set Build Command: `npm install`, Start Command: `node server.js`
5. Add environment variables from `.env.example`
6. Deploy → copy the service URL (e.g., `https://tuition-saas.onrender.com`)

### Frontend → Vercel
1. Push frontend folder to GitHub
2. Go to [vercel.com](https://vercel.com) → New Project
3. Import your repo
4. Set environment variable: `REACT_APP_API_URL=https://your-render-url/api`
5. Deploy

---

## 🔌 API Endpoints

### Auth
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/register` | Register new tutor |
| POST | `/api/auth/login` | Login and get JWT |
| GET | `/api/auth/me` | Get current user |

### Students (Protected)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/students` | Get all students |
| POST | `/api/students` | Add student |
| PUT | `/api/students/:id` | Update student |
| DELETE | `/api/students/:id` | Soft delete student |

### Attendance (Protected)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/attendance/today` | Today's attendance summary |
| POST | `/api/attendance/bulk` | Bulk mark attendance |
| GET | `/api/attendance/:studentId` | Student attendance |
| POST | `/api/attendance/:studentId` | Mark attendance |

### Fees (Protected)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/fees/summary?month=YYYY-MM` | Fee summary |
| PUT | `/api/fees/:studentId/:month` | Update fee status |
| POST | `/api/fees/remind/:studentId` | Send WhatsApp reminder |

### Analytics (Protected)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/analytics` | Full analytics data |

---

## 📱 Features

- **Multi-tutor SaaS** — each tutor manages their own isolated data via JWT auth
- **Student Management** — add, view, delete students with full details
- **Daily Attendance** — mark present/absent per student, bulk actions, date picker
- **Fee Tracking** — auto-generated monthly fees from joining date, toggle paid/unpaid
- **WhatsApp Reminders** — send fee reminders via Twilio (manual + automated cron)
- **Analytics** — fee trend charts, students by class/school, attendance rates
- **Cron Job** — daily 9 AM automatic fee reminder for unpaid students

---

## 🔧 Twilio WhatsApp Setup

1. Create a Twilio account at [twilio.com](https://twilio.com)
2. Activate the WhatsApp Sandbox under Messaging → Try it Out
3. Save Sandbox number (e.g., `whatsapp:+14155238886`)
4. Copy `TWILIO_ACCOUNT_SID`, `TWILIO_AUTH_TOKEN`
5. For production: apply for a WhatsApp Business number

---

## 🛡️ Security

- Passwords hashed with bcryptjs (12 rounds)
- JWT tokens (7-day expiry)
- All student routes protected by JWT middleware
- Data isolation: users only access their own students
- Environment variables for all secrets

---

## 🧱 Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18, Tailwind CSS, Recharts, Axios |
| Backend | Node.js, Express.js |
| Database | MongoDB + Mongoose |
| Auth | JWT + bcryptjs |
| Notifications | Twilio WhatsApp API |
| Scheduling | node-cron |
| Frontend Deploy | Vercel |
| Backend Deploy | Render.com |
| DB Hosting | MongoDB Atlas |
