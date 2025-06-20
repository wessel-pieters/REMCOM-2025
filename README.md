# REMCOM-2025
Deliberate practice version of REMCOM using AI

# REMCOM MVP Monorepo

## Structure
- `backend/` — Spring Boot (Java)
- `frontend/` — React (TypeScript)

## Prerequisites
- Docker & Docker Compose
- Node.js (for frontend dev)
- Java 17+ & Maven/Gradle (for backend dev)

## Quick Start (Dev)
```bash
git clone <repo-url>
cd REMCOM-2025
docker-compose up --build
```

- Backend: http://localhost:8080
- Frontend: http://localhost:3000
- PostgreSQL: localhost:5432 (user/pass: remcom/remcom)

## Local Dev (Optional)
- See `backend/` and `frontend/` folders for standalone dev instructions.
