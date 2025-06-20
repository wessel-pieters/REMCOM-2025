# REMCOM MVP Specification

## Objective
Deliver a streamlined, web-based enforcement tool that allows roadside officials to manually process and reconcile Warrants of Arrest (WoAs) and outstanding notices for vehicles and drivers—without relying on ANPR, handheld devices, or mobile scanners.

---

## MVP Scope & Features

### 1. Manual Data Entry Interface
- Officers can manually input:
  - Driver ID number
  - Vehicle registration number

### 2. Centralised WoA/Notice Lookup
- Query and display all outstanding notices or WoAs linked to the entered ID or vehicle.
- Clearly distinguish between driver-linked and vehicle-linked entries.

### 3. Actionable Enforcement Interface
- Officers can mark:
  - WoA as executed
  - Fine or notice as paid
- Editing allowed only with authenticated user sessions.
- All actions logged with immutable audit trail.

### 4. Receipt and Warrant Issuance
- Generate and display printable PDFs:
  - Payment receipts
  - Executed warrants

### 5. TRAFMAN Integration (Basic Export)
- Generate a daily transaction file in XML format for manual upload into TRAFMAN.
- Include all available data: payments, executions, timestamps, user IDs.

### 6. Admin and Reconciliation Module
- Admins can:
  - View and verify logs of payments and WoA executions
  - Manually upload reconciliation XML files
  - Detect and resolve mismatches (e.g., duplicates, amount discrepancies)

---

## Non-Functional Requirements

- **Authentication**: Username/password login, session expires after 30 minutes
- **Audit Logging**: Immutable logs of all actions (who, what, when)
- **Performance**: Offline-first support deferred to future enhancement
- **Security**: HTTPS-only, encrypted sensitive data, OWASP top 10 compliance

---

## Out of Scope for MVP

- ANPR integration
- Handheld/PDA scanning or apps
- Real-time mobile sync
- Automatic vehicle redirection flow
- Payment gateway integration (all payments recorded as manual)

---

## User Roles

### Officer
- Manual data entry
- View notices and WoAs
- Mark WoAs as executed
- Mark notices as paid
- Generate PDFs
- Cannot access reconciliation module

### Admin
- Access reconciliation module
- View logs
- Upload reconciliation XML files
- Track and resolve mismatches

---

## Lookup Results Display

### Warrants of Arrest (WoA)
- Case Number
- Appearance Date
- Charge
- Date of Issue
- Status

### Notices
- Offence
- Date
- Fine Amount
- Status

Grouped by driver-linked and vehicle-linked entries.

---

## Enforcement Action Logging
- Automatically logs:
  - Officer ID (from session)
  - Timestamp

---

## PDF Templates

### Payment Receipt
- Amount Due
- Amount Paid
- Payment Date
- Notice Number
- Offence
- Driver Details

### Executed Warrant
- Case Number
- Appearance Date
- Charge
- Date of Issue
- Date of Execution
- Court Name

---

## TRAFMAN XML Export
- Includes all available data per transaction:
  - Transaction Type
  - Timestamp
  - Officer/User ID
  - Notice Number / Case Number
  - Offence / Charge
  - Fine Amount / Amount Paid
  - Payment Date / Execution Date
  - Driver ID
  - Vehicle Registration Number
  - Court Name (for WoAs)

---

## Reconciliation Module

### Mismatch Detection
- Compare uploaded XML files with internal logs
- Detect:
  - Duplicate entries
  - Payment amount discrepancies

### Display
- List view with:
  - Transaction reference
  - Field with discrepancy
  - Internal vs. XML value
  - Status
- Admin actions:
  - Download report
  - Flag for review
  - Mark as resolved with note

---

## Sorting & Navigation
- Sort by:
  - Date
  - Status

---

## UI/UX Requirements
- Color Scheme: Chrome-based theme
- Language: English only
- Accessibility: No specific requirements for MVP

---

## Deployment & Environment

- **Frontend**: React, TypeScript, npm, MUI, React Router
- **Backend**: Spring, Spring Data JPA, latest LTE version
- **Database**: PostgreSQL
- **Migrations**: Liquibase
- **Authentication**: JWT tokens
- **API Documentation**: Swagger
- **Containerization**: Docker (frontend and backend)
- **Browser Compatibility**: Google Chrome
- **Concurrency**: Supports multiple concurrent users

---

## Audit Logging
- All actions logged with:
  - Who (User ID)
  - What (Action)
  - When (Timestamp)
- Logs are immutable and securely stored
