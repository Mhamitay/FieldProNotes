RiseFSM is a **Field Service Management (FSM)** system built using **ASP.NET Core 6 Razor Pages**, following a **multi-tenant architecture** and **domain-driven design**.

---

## 🧱 Project Structure

### 📁 Domain Layer
Located in `Web/Domain`, this layer contains domain model classes with encapsulated **business logic** and **data access**.

#### Base Patterns:
- `DataModelBase`: Utility methods for data processing.
- `DomainBase`: Extended base class with common functionality.
- `ChangesDomainBase<T>`: Tracks changes to domain models.

---

### 📁 Models Layer
Defined in `Web/Models`, these classes represent data structures used across the system.

#### Base Models:
- `TenantBaseModel`: Adds multi-tenant support.
- `CodeTableTenantBaseModel`: For code/lookup tables.
- `TenantOfflineBase`: Enables offline data tracking.
- `DatesBase`: Adds date-tracking capability (e.g., used in `JobUser`).

---

### 🗄️ Data Access
The application uses **ADO.NET** (not Entity Framework), relying on **stored procedures**.

#### Key Points:
- Custom `DB` class handles DB connections and transactions.
- SQL stored procedures accessed through constants in `SP` class.
- Tenant isolation enforced via stored procedure parameters.

---

### 🖥️ UI Layer
Built with **Razor Pages** and **JavaScript/TypeScript** for responsive field operation support.

#### Features:
- Uses **DevExtreme** UI components.
- Built-in **report generation**.
- **Offline support** for field users.

---

## 👤 JobUser Class Details

`JobUser` represents the association of **users to jobs**, managing assignments, shifts, and bonuses.

### 1. Core Functionality
- Assign users to jobs with `JobID`, `JobUserID`, and `JobShiftID`.
- Manage scheduled dates via `Dates`.
- Track and calculate bonuses:
  - `BonusDefault`, `BonusPaid`, `BonusPaidAmount`
- Handle **offline operations** (`InsertOffline`, etc.)

### 2. Database Operations
- `SelectAll`: Retrieves all users assigned to a job.
- `SelectOne`: Gets a single `JobUser` record.
- `Insert`, `Update`, `Delete`: Standard CRUD operations.
- `InsertOffline`: Handles offline assignments.

### 3. Data Validation
- Ensures required fields are present.
- Validates:
  - Date ranges
  - Bonus values
  - Operation-specific rules

### 4. Model Structure
- Based on `Models.JobUser`, which extends `DatesBase`.
- Tracks:
  - Job assignment data
  - Shift IDs
  - Bonus and date details
- Methods:
  - `RefreshDaysList`
  - `GetDaysString`

---

## 🌐 Multi-Tenant Architecture

Designed for **scalable, isolated tenant management**.

- All operations pass `_tenantID` for isolation.
- `_userID` used for auditing and security context.
- Stored procedures ensure tenant data boundaries.

---

## 🔁 Common Patterns

Consistent architecture supports maintainability and extensibility:

- Stored procedure-based data access.
- Input validation before DB operations.
- Error handling and propagation to UI.
- Full support for online + offline workflows.
- Extended code tables used for dropdowns and lookups.
- Transactional DB operations (Begin/Commit/Rollback).

---

## ✅ Summary

RiseFSM offers a robust structure for **multi-tenant field service management** with **offline operation support**, optimized for real-world field conditions. It emphasizes:

- Clean separation of concerns (domain, models, UI).
- Strong data integrity and security.
- Enterprise patterns for scalable growth.

---

> _Tags: #RiseFSM #ProjectOverview #FSM #MultiTenant #OfflineSupport #JobUser #Architecture_
