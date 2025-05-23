
# REMCOM MVP Specification Document

---

## 1. Project Overview

**Objective:**  
Deliver a streamlined, web-based enforcement tool that allows roadside officials to manually process and reconcile Warrants of Arrest (WoAs) and outstanding notices for vehicles and drivers — without relying on ANPR, handheld devices, or mobile scanners.

**Scope:**  
Manual data entry, querying outstanding warrants and notices, updating statuses, generating PDFs, daily XML exports for TRAFMAN, and reconciliation modules with admin controls.

---

## 2. Technology Stack

- **Frontend:** Angular, CSS, Bootstrap (responsive design)  
- **Backend:** Java Spring Boot  
- **Database:** PostgreSQL  
- **Hosting:** On-premises, Docker containers  
- **Authentication:** JWT tokens with username/password login, no MFA  
- **Timezone:** Server local time for all timestamps  
- **Language:** English only

---

## 3. User Roles & Permissions

| Role   | Permissions                                                                                  |
|--------|----------------------------------------------------------------------------------------------|
| Officer| Manual data input, querying WoAs/notices, updating status, generating/displaying PDFs, generate daily XML export. |
| Admin  | All Officer permissions plus user management (create/edit/deactivate users), view audit logs, upload reconciliation files, review mismatches, add comments. |

---

## 4. Data Model

### 4.1 Driver  
| Field          | Data Type       | Description                         |  
|----------------|-----------------|-----------------------------------|  
| DriverID       | Integer / UUID  | Unique driver identifier           |  
| FirstName      | String          | Driver’s first name                |  
| LastName       | String          | Driver’s last name                 |  
| DateOfBirth    | Date            | Driver’s date of birth             |  
| Nationality    | String          | Driver’s nationality               |  
| IDNumber       | String          | National or passport ID number    |  
| VehicleID      | Integer / UUID  | Reference to vehicle belonging to driver |

### 4.2 Vehicle  
| Field           | Data Type      | Description                       |  
|-----------------|----------------|---------------------------------|  
| VehicleID       | Integer / UUID | Unique vehicle identifier         |  
| RegistrationNumber | String       | Vehicle license plate/registration number |  
| Make            | String         | Manufacturer                     |  
| Model           | String         | Vehicle model                   |  
| Year            | Integer        | Year manufactured               |  
| Color           | String         | Vehicle color                   |  
| VehicleType     | String         | Category/type (Truck, Sedan, etc.) |

### 4.3 Warrant of Arrest (WoA)  
| Field           | Data Type       | Description                         |  
|-----------------|-----------------|-----------------------------------|  
| WarrantID       | Integer / UUID  | Unique warrant identifier          |  
| WarrantNumber   | String          | Official warrant reference         |  
| IssuedDate      | DateTime        | Date/time warrant issued           |  
| CourtName       | String          | Issuing court name                 |  
| CaseNumber      | String          | Linked court case number           |  
| PersonID        | Integer / UUID  | Reference to driver                |  
| OffenseType     | String          | Offense category/type              |  
| OffenseDescription | Text          | Detailed offense description       |  
| Status          | String          | Current status (e.g., Active, Executed) |  
| ExecutionDate   | DateTime (nullable) | Date/time warrant executed (if applicable) |  
| ExecutedBy     | Integer / UUID  | Officer who executed warrant (if applicable) |  
| IsActive        | Boolean         | Whether warrant is active          |

### 4.4 Notice  
| Field           | Data Type       | Description                         |  
|-----------------|-----------------|-----------------------------------|  
| NoticeID        | Integer / UUID  | Unique notice/fine identifier      |  
| NoticeNumber    | String          | Official notice reference          |  
| IssueDate       | DateTime        | Date/time notice issued            |  
| PersonID        | Integer / UUID  | Reference to driver                |  
| OffenseCode     | String          | Offense category/code              |  
| OffenseDescription | Text          | Detailed offense description       |  
| Location        | String          | Offense location                   |  
| FineAmount      | Decimal         | Fine amount                       |  
| DiscountAmount  | Decimal (nullable) | Early payment discount (optional) |  
| DueDate         | Date            | Payment due date                   |  
| PaymentStatus   | String          | Status (Unpaid, Paid, Disputed, etc.) |  
| PaymentDate     | DateTime (nullable) | Date fine was paid (if applicable) |  
| LinkedWarrantID | Integer / UUID (optional) | Reference to linked warrant if any |  
| IssuedBy        | Integer / UUID  | Issuing authority/officer         |

---

## 5. Status Values

### WoA Statuses  
Issued, Active, Pending Execution, Executed, Cancelled, Expired, Withdrawn, Suspended, Resolved, Referred to Collections, Contested

### Notice Statuses  
Issued, Delivered, Acknowledged, Unpaid, Paid, Partially Paid, Overdue, In Dispute, Resolved, Withdrawn, Cancelled, Referred to Collections, Linked to Warrant, Suspended

---

## 6. Functional Requirements

### 6.1 Manual Data Entry & Search  
- Allow Officers to input Driver ID number or Vehicle registration number (partial matches allowed).  
- Search results show a combined list of all outstanding WoAs and Notices, grouped into Driver-linked and Vehicle-linked sections.  
- Filters available on search screen: Date range (issue/issued dates) and Status.

### 6.2 Results Display  
- Show key info per record (Warrant/Notice numbers, offense type, issue date, status, fines, court name).  
- Allow Officers to mark WoA as executed or Notice as paid.  
- Enable editing only during authenticated sessions with audit trail.  
- Prevent concurrent edits with optimistic locking and conflict detection.

### 6.3 PDF Generation  
- Generate and display printable PDFs for:  
  - Payment Receipts (include ref number, driver/vehicle info, offense, amount paid, officer info, date/time, confirmation statement)  
  - Executed Warrants (similar minimal details).  
- Save generated PDFs in system on first generation; retrieve saved copies on subsequent views.  
- Officers and Admins can search/filter stored PDFs by date, reference number, user ID.

### 6.4 TRAFMAN Integration  
- Generate a daily XML transaction file containing all payments and executions from the previous day, including timestamps and user IDs.  
- Suggested XML format: each transaction includes type, reference ID, timestamp, user ID, and payment amount (if applicable).  
- Manual upload by Officers or Admins into TRAFMAN system.

### 6.5 Admin & Reconciliation Module  
- Admins view logs of payments, executions, and reconciliation uploads.  
- Manually upload reconciliation XML files.  
- System flags mismatches in a simple report interface.  
- Admins can add notes/comments on mismatches.  
- No automated notifications or complex workflows.

### 6.6 User Management  
- Admins can create, edit, deactivate user accounts.  
- Roles: Officer and Admin with corresponding permissions.

---

## 7. Non-Functional Requirements

- **Authentication & Authorization:** JWT tokens with username/password login; no MFA.  
- **Audit Logging:** Log user actions with user ID, timestamp for key events (status changes, login/logout, edits, file generations, uploads). Logs accessible only by Admins, read-only.  
- **Soft Deletes:** No physical deletions; records have IsActive/IsDeleted flags.  
- **Performance:** Designed for moderate use, no specific concurrency beyond optimistic locking.  
- **Security:** HTTPS-only, encrypted sensitive data, OWASP top 10 compliance.  
- **UI:** Responsive design, English only.

---

## 8. Error Handling

- Validation errors on user inputs (e.g., ID number format, required fields).  
- Conflict detection with clear user messages on concurrency conflicts.  
- Graceful error pages on system faults with friendly messages.  
- Log errors internally with timestamps and user context for troubleshooting.

---

## 9. Testing Plan

### 9.1 Unit Testing  
- Backend services, data validation, authentication, authorization, audit logging.  
- Frontend components, search, forms, filtering, concurrency controls.

### 9.2 Integration Testing  
- End-to-end flows: search → update → PDF generation → XML export.  
- User management flows.  
- Admin reconciliation and log review.

### 9.3 User Acceptance Testing (UAT)  
- Verify UI/UX flows match spec.  
- Confirm role-based access and permissions.  
- Test concurrency conflict scenarios.  
- Test PDF generation and retrieval.  
- Validate TRAFMAN XML file format and content.  
- Confirm reconciliation mismatch report correctness.

### 9.4 Security Testing  
- Test authentication and authorization enforcement.  
- Verify audit log immutability and access restrictions.  
- Conduct OWASP Top 10 vulnerability scans.

---

## 10. Deployment & Environment

- Containerized backend and frontend services via Docker.  
- PostgreSQL database hosted on-premises.  
- Secure HTTPS setup.  
- Environment configurations for dev, test, and production.

---

