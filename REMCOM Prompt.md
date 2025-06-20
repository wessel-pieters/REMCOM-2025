# REMCOM MVP: Step-by-Step Blueprint & Prompts

## 1. High-Level Blueprint

### 1.1. Foundation
- Set up monorepo structure (frontend, backend, shared)
- Initialize Git, Docker, CI/CD skeleton
- Define environments (dev, test, prod)

### 1.2. Backend
- Scaffold Spring Boot project with PostgreSQL, Liquibase, JWT, Swagger
- Define core domain models: User, Notice, Warrant, AuditLog
- Implement authentication (JWT, password hashing)
- Implement CRUD for Notices and Warrants
- Implement immutable audit logging
- Implement PDF generation endpoints
- Implement TRAFMAN XML export
- Implement reconciliation logic and endpoints

### 1.3. Frontend
- Scaffold React/TypeScript app with MUI, React Router
- Implement authentication flow (login, session expiry)
- Implement manual data entry UI
- Implement lookup and results display
- Implement enforcement actions (mark as paid/executed)
- Implement PDF viewing/printing
- Implement admin/reconciliation module
- Implement sorting, navigation, and export

### 1.4. Integration & Testing
- End-to-end wiring (API, UI, DB)
- Unit, integration, and E2E tests
- Dockerize and verify deployment

---

## 2. Iterative Chunks

### Chunk 1: Project & Environment Setup
- Monorepo structure
- Backend Spring Boot skeleton
- Frontend React skeleton
- Docker Compose for local dev
- CI/CD skeleton

### Chunk 2: Authentication & User Management
- User entity/model
- JWT authentication (backend)
- Login UI (frontend)
- Session expiry logic

### Chunk 3: Core Domain Models & Persistence
- Notice, Warrant, AuditLog entities
- Liquibase migrations
- Repository interfaces

### Chunk 4: Manual Data Entry & Lookup
- Backend: Notice/Warrant lookup endpoints
- Frontend: Manual entry form, results display

### Chunk 5: Enforcement Actions & Audit Logging
- Backend: Mark as paid/executed endpoints
- Audit logging logic
- Frontend: Action buttons, confirmation, feedback

### Chunk 6: PDF Generation
- Backend: PDF endpoints (receipt, warrant)
- Frontend: PDF viewing/printing

### Chunk 7: TRAFMAN XML Export
- Backend: XML export endpoint
- Frontend: Download/export UI

### Chunk 8: Reconciliation Module
- Backend: XML upload, mismatch detection, admin actions
- Frontend: Admin UI, mismatch list, actions

### Chunk 9: Sorting, Navigation, Polish
- Sorting/filtering on UI
- Final UI/UX tweaks
- Accessibility, browser compatibility

### Chunk 10: Testing & Deployment
- Unit/integration/E2E tests
- Docker build/test
- Deployment scripts

---

## 3. Break Down Chunks into Small Steps

### Example: Chunk 2 (Authentication & User Management)

#### Step 2.1: Backend User Entity & Repository
#### Step 2.2: Backend JWT Auth Filter & Config
#### Step 2.3: Backend Auth Controller (login, session)
#### Step 2.4: Frontend Login Page (form, validation)
#### Step 2.5: Frontend Auth Context (JWT storage, expiry)
#### Step 2.6: Protect routes, redirect on expiry

---

## 4. Prompts for Each Step

### Chunk 1: Project & Environment Setup

#### Prompt 1.1
```text
Scaffold a monorepo with two main folders: `backend` (Spring Boot, Gradle/Maven) and `frontend` (React, TypeScript, npm). Add a root-level `docker-compose.yml` that starts both services and a PostgreSQL database. Include a `.gitignore` for Java, Node, and Docker artifacts. Add a `README.md` with setup instructions.
```

#### Prompt 1.2
```text
In the `backend` folder, scaffold a Spring Boot project with dependencies: Spring Web, Spring Data JPA, PostgreSQL, Liquibase, Spring Security, jjwt, and Swagger/OpenAPI. Configure application properties for PostgreSQL connection and enable Swagger UI.
```

#### Prompt 1.3
```text
In the `frontend` folder, scaffold a React app using TypeScript, MUI, React Router, and Axios. Set up a basic folder structure: `components`, `pages`, `api`, `contexts`. Add a placeholder home page and login page.
```

#### Prompt 1.4
```text
Create a `docker-compose.yml` at the root that starts the backend (on port 8080), frontend (on port 3000), and PostgreSQL (on port 5432). Ensure the backend waits for the database to be ready before starting.
```

---

### Chunk 2: Authentication & User Management

#### Prompt 2.1
```text
In the backend, define a `User` JPA entity with fields: id, username, password (hashed), role (OFFICER or ADMIN), and createdAt. Create a `UserRepository` interface. Add a Liquibase migration to create the users table.
```

#### Prompt 2.2
```text
Implement JWT-based authentication in the backend. Add a login endpoint that verifies username/password, issues a JWT token (with 30 min expiry), and returns it. Add a JWT filter that protects all endpoints except `/login` and `/swagger-ui/**`.
```

#### Prompt 2.3
```text
On the frontend, implement a login page with username/password fields and validation. On submit, call the backend login endpoint, store the JWT in memory (not localStorage), and redirect to the main app on success. Show error messages on failure.
```

#### Prompt 2.4
```text
Create a React context for authentication state. Store the JWT and user info in context. Implement automatic logout when the JWT expires (30 min). Protect all routes except `/login` and redirect to login if not authenticated.
```

---

### Chunk 3: Core Domain Models & Persistence

#### Prompt 3.1
```text
In the backend, define JPA entities for `Notice`, `Warrant`, and `AuditLog` according to the spec. Add fields for all required data (see spec). Create repositories and Liquibase migrations for each table.
```

#### Prompt 3.2
```text
Implement backend endpoints to create, read, and update Notices and Warrants. Ensure that only authenticated users can perform actions, and only admins can access reconciliation endpoints.
```

#### Prompt 3.3
```text
Write unit tests for the Notice and Warrant repositories and service layers. Test CRUD operations and basic validation.
```

---

### Chunk 4: Manual Data Entry & Lookup

#### Prompt 4.1
```text
Implement a backend endpoint to look up all outstanding Notices and Warrants by driver ID or vehicle registration. Return results grouped by driver-linked and vehicle-linked entries.
```

#### Prompt 4.2
```text
On the frontend, create a manual data entry form for officers to input driver ID or vehicle registration. On submit, call the lookup endpoint and display grouped results in a table, clearly distinguishing driver-linked and vehicle-linked entries.
```

#### Prompt 4.3
```text
Write frontend unit tests for the data entry form and results display. Test validation, API call, and grouping logic.
```

---

### (Continue breaking down each chunk similarly...)

---

## 5. Review & Right-Sizing

- Each prompt is focused, testable, and builds on previous steps.
- No orphaned code: each step integrates with the previous.
- Early steps focus on scaffolding and authentication, enabling secure development.
- Each domain model and feature is implemented incrementally, with tests.
- PDF and XML features are added after core CRUD and audit logging.
- Admin/reconciliation is layered on top, after officer flows are stable.
- Final steps focus on polish, sorting, and deployment.

---

## 6. Summary

This plan ensures:
- **Incremental progress**: Each step is small, testable, and builds on the last.
- **Best practices**: Security, testing, and integration are prioritized.
- **No big jumps**: Complexity is introduced gradually.
- **No orphaned code**: Every prompt results in integrated, working code.

---

## 7. Next Steps

Use the above prompts, in order, to drive code generation and implementation. Each prompt can be expanded with more detail as needed, but this structure will keep the project moving forward safely and efficiently.
