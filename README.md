# WorkNest — Secure Workspace & Project Management Platform

WorkNest is a modern, enterprise-grade full-stack workspace management platform that unifies project & task tracking (TaskFlow concept) with encrypted, role-governed document storage and sharing (CloudVault concept).

---

## 🌟 Key Features

### 1. 🏢 Workspaces & Access Control
- **Multi-Tenant Workspaces**: Segment projects, documents, and audit logs by department or team.
- **Granular Roles**: Workspace Owners, Admins, and Members with permission checks across all API endpoints.
- **Team Management**: Invite colleagues, update permissions, or transfer ownership with real-time feedback.

### 2. 📋 Project & Task Management (TaskFlow)
- **Projects**: Dedicated project spaces with custom color accents and workspace isolation.
- **Interactive Kanban Board**: Visual workflow stages (`To Do`, `In Progress`, `Completed`) with instant status transitions.
- **Table / List View**: Sortable, filterable list with priority badges (`Low`, `Medium`, `High`, `Urgent`), assignee avatars, and overdue warnings.
- **Task Filtering**: Query by project, status, priority, or assignee.

### 3. 🔐 Secure File Storage & Sharing (CloudVault)
- **Secure File Storage**: SHA-256 deduplicated, server-side encrypted disk storage with UUID-based path isolation preventing path traversal.
- **Granular File Sharing**: Share files with specific colleagues with `Viewer` (read/download) or `Editor` permissions.
- **Direct Downloads**: Authenticated file streaming with token validation and proper MIME-type headers.
- **File Types**: Built-in visual icons and previews for PDFs, Word docs, spreadsheets, code files, images, videos, audio, and archives.

### 4. 📊 Dashboard & Audit Trails
- **Executive Analytics**: Real-time summary cards for total workspaces, active projects, task completion rates, and file counts.
- **Progress Trackers**: Visual completion percentage and progress bars.
- **Audit Logs**: Comprehensive event tracking (logins, creates, updates, deletes, uploads, shares) with timestamps and user attribution.

### 5. 🛡️ Security & Authentication
- **Argon2 Password Hashing**: Modern, memory-hard hashing resistant to GPU cracking.
- **JWT (JSON Web Tokens)**: Stateless authentication with configurable expiration.
- **CORS & Rate Protection**: Controlled cross-origin access and request size limits.

---

## 🛠️ Technology Stack

| Layer | Technologies |
|---|---|
| **Backend** | Python 3.11, FastAPI, SQLAlchemy 2.0, Alembic, Pydantic v2, Pytest |
| **Frontend** | React 18, TypeScript, Vite, React Router v6, Axios, Lucide Icons, Vanilla CSS Design System |
| **Database** | PostgreSQL 16 (production/docker), SQLite supported for testing |
| **Containerization** | Docker, Docker Compose, Nginx reverse proxy |
| **CI / CD** | GitHub Actions Automated Testing & Build Validation |

---

## 🚀 Quickstart with Docker Compose

Ensure [Docker Desktop](https://www.docker.com/products/docker-desktop/) is installed and running, then execute:

```bash
# Clone the repository and navigate to root
cd Worknest

# Start all services (Database, Backend, Frontend)
docker compose up --build
```

Once started:
- 🌐 **Frontend Application**: [http://localhost:3000](http://localhost:3000)
- 🔌 **Backend REST API**: [http://localhost:8000](http://localhost:8000)
- 📖 **Interactive Swagger API Docs**: [http://localhost:8000/docs](http://localhost:8000/docs)
- 📑 **ReDoc Documentation**: [http://localhost:8000/redoc](http://localhost:8000/redoc)

---

## 💻 Local Development Setup

### Backend (FastAPI)

1. Navigate to the backend directory:
   ```bash
   cd backend
   ```

2. Create and activate a Python virtual environment:
   ```bash
   python -m venv venv
   # On Windows (PowerShell):
   .\venv\Scripts\Activate.ps1
   # On Linux/macOS:
   source venv/bin/activate
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

4. Run database migrations:
   ```bash
   alembic upgrade head
   ```

5. Run test suite:
   ```bash
   pytest -v
   ```

6. Start backend development server:
   ```bash
   uvicorn app.main:app --reload --port 8000
   ```

### Frontend (React + TypeScript)

1. Navigate to the frontend directory:
   ```bash
   cd frontend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Start Vite dev server:
   ```bash
   npm run dev
   ```

4. Access the development app at `http://localhost:5173`.

---

## 🧪 Testing

The backend includes a comprehensive automated test suite covering:
- **Authentication**: Registration, login, duplicate checks, profile update, password change.
- **Workspaces**: CRUD operations, multi-tenancy, member role assignments, role escalation checks.
- **Projects**: CRUD operations, workspace boundaries.
- **Tasks**: CRUD operations, status transitions, priority filtering, pagination.
- **Files**: Upload validation, size limits, sharing with permissions, download headers, rename, deletion.

To run the automated tests:
```bash
cd backend
pytest -v
```

---

## 📂 Project Architecture

```
Worknest/
├── backend/
│   ├── app/
│   │   ├── api/             # FastAPI routers (auth, workspaces, projects, tasks, files, dashboard, activity)
│   │   ├── core/            # Config, security, JWT, Argon2 hashing
│   │   ├── db/              # SQLAlchemy session & base models
│   │   ├── models/          # Database ORM models
│   │   ├── schemas/         # Pydantic validation schemas
│   │   ├── services/        # Business logic & file system storage
│   │   └── main.py          # FastAPI application initialization & middleware
│   ├── migrations/          # Alembic database migration scripts
│   ├── tests/               # Pytest test suite (auth, workspaces, projects, tasks, files)
│   ├── Dockerfile           # Backend container image definition
│   └── requirements.txt     # Python dependencies
│
├── frontend/
│   ├── src/
│   │   ├── components/      # UI components (Sidebar, AppLayout, Modal)
│   │   ├── hooks/           # useAuth, useToast custom hooks
│   │   ├── pages/           # Dashboard, Workspaces, Projects, Tasks, Files, Shared, Activity, Auth
│   │   ├── services/        # Axios API client & typed endpoints
│   │   ├── types/           # TypeScript data models and interfaces
│   │   ├── App.tsx          # Router configuration
│   │   ├── main.tsx         # React root mounting
│   │   └── index.css        # Premium dark glassmorphic design system
│   ├── nginx.conf           # Production Nginx reverse proxy configuration
│   ├── Dockerfile           # Multi-stage production Docker build
│   └── package.json         # Frontend dependencies and build scripts
│
├── .github/workflows/       # CI/CD automated pipeline
├── docker-compose.yml       # Production Docker Compose orchestration
├── .env.example             # Environment variables template
└── README.md                # Project documentation
```

---

## 🔒 Security Highlights

- **Non-Root Docker Containers**: Backend runs under an unprivileged `worknest` system user.
- **SQL Injection Prevention**: 100% parameterized queries via SQLAlchemy 2.0 ORM.
- **File Validation**: Strict file extension whitelisting and file size capping (default 50MB).
- **Sanitized Filenames**: Stored files use random UUID hashes on disk; original filenames preserved safely in metadata tables.
- **Authentication Guards**: Every protected route verifies signature, expiration, and user active status via Bearer JWT.

---

## 📄 License

This project is licensed under the MIT License.
