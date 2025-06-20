# REMCOM MVP Development Checklist

## Foundation & Setup
- [x] Create monorepo structure: `backend/`, `frontend/`, `shared/`
- [x] Initialize Git repository
- [x] Add `.gitignore` for Java, Node, Docker
- [x] Add root `README.md` with setup instructions
- [x] Set up Docker Compose for backend, frontend, PostgreSQL
- [ ] Set up CI/CD skeleton (optional for MVP)

## Backend: Spring Boot
- [ ] Scaffold Spring Boot project with Gradle/Maven
- [ ] Add dependencies: Spring Web, Spring Data JPA, PostgreSQL, Liquibase, Spring Security, jjwt, Swagger
- [ ] Configure PostgreSQL connection
- [ ] Enable Swagger UI
- [ ] Set up Liquibase for DB migrations

### Authentication & User Management
- [ ] Define `User` entity (id, username, password, role, createdAt)
- [ ] Create `UserRepository`
- [ ] Add Liquibase migration for users table
- [ ] Implement JWT authentication (login endpoint, JWT filter)
- [ ] Secure endpoints (JWT required except `/login`, `/swagger-ui/**`)
- [ ] Add password hashing

### Core Domain Models
- [ ] Define `Notice` entity
- [ ] Define `Warrant` entity
- [ ] Define `AuditLog` entity
- [ ] Create repositories for each
- [ ] Add Liquibase migrations for all tables

### Business Logic & Endpoints
- [ ] CRUD endpoints for Notices
- [ ] CRUD endpoints for Warrants
- [ ] Lookup endpoint (by driver ID or vehicle reg)
- [ ] Mark Notice as paid endpoint
- [ ] Mark Warrant as executed endpoint
- [ ] Implement immutable audit logging for all actions
- [ ] PDF generation endpoints (payment receipt, executed warrant)
- [ ] TRAFMAN XML export endpoint
- [ ] Reconciliation endpoints (XML upload, mismatch detection, admin actions)

### Testing
- [ ] Unit tests for repositories
- [ ] Unit tests for services
- [ ] Integration tests for endpoints

## Frontend: React/TypeScript
- [ ] Scaffold React app with TypeScript, MUI, React Router, Axios
- [ ] Set up folder structure: `components/`, `pages/`, `api/`, `contexts/`
- [ ] Add placeholder home and login pages

### Authentication
- [ ] Implement login page (form, validation, error handling)
- [ ] Store JWT in React context (not localStorage)
- [ ] Implement session expiry (30 min)
- [ ] Protect routes (redirect to login if not authenticated)

### Officer Flows
- [ ] Manual data entry form (driver ID, vehicle reg)
- [ ] Call lookup endpoint and display grouped results
- [ ] Action buttons to mark as paid/executed
- [ ] Confirmation and feedback for actions
- [ ] PDF viewing/printing for receipts and warrants

### Admin Flows
- [ ] Admin-only reconciliation module
- [ ] Upload reconciliation XML
- [ ] Display mismatches (list, details)
- [ ] Admin actions: download report, flag for review, mark as resolved

### UI/UX
- [ ] Chrome-based color scheme
- [ ] Sorting and filtering (date, status)
- [ ] Navigation (React Router)
- [ ] Responsive layout

### Testing
- [ ] Unit tests for forms and components
- [ ] Integration tests for flows

## Integration & Deployment
- [ ] End-to-end wiring (API, UI, DB)
- [ ] Dockerize backend and frontend
- [ ] Verify local deployment with Docker Compose
- [ ] Finalize deployment scripts

## Final Polish
- [ ] Review for security (OWASP top 10)
- [ ] Ensure HTTPS-only configuration
- [ ] Test with multiple concurrent users
- [ ] Review audit logging for completeness
- [ ] Final browser compatibility check (Chrome)

---

Check off each item as you complete it. Add more detail as needed for your workflow!
