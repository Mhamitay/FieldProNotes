A comprehensive analysis of the **RiseFSM** project based on provided code and context.

---

## 🏗️ Project Architecture

### Multi-Tenant Design
- Each tenant's data is isolated at the database level.
- All operations include tenant ID (`_tenantID`) as a parameter.
- User context (`_userID`) is tracked for security and auditing.

### Layered Architecture
- **Domain Layer**: Business logic and data access (e.g., `JobUser.cs`).
- **Models Layer**: Defines data structures.
- **Services Layer**: Utilities and cross-cutting concerns.
- **API Layer**: REST endpoints for CRUD operations.
- **UI Layer**: Razor Pages for the web interface.

### Data Access Pattern
- Direct ADO.NET with stored procedures (not using Entity Framework).
- Custom DB class for transaction management and command execution.
- Strong typing in C# with model validation.
- Security enforced at stored procedure level with tenant parameters.

### Online/Offline Support
- Methods like `InsertOffline` indicate offline functionality.
- Sync mechanisms for limited-connectivity field operations.
- `JobIds` string parameter used for batch sync operations.

---

## 📦 Domain Models

### Base Models
- `DataModelBase`: Utility methods for data processing.
- `DatesBase`: Adds date-tracking.
- `TenantBaseModel`: Multi-tenant support.
- `TenantOfflineBase`: Offline functionality.

### Special Models
- `CodeTableTenantBaseModel`: For lookup/code tables.
- `ChangesDomainBase<T>`: Models that track changes.
- `IOfflineBase`: Interface for offline-capable models.

---

## 👤 JobUser Class Analysis

Manages associations between users and jobs, shift tracking, and bonuses.

### 1. User Assignment Management
- Links users to jobs via `JobUserID` and `JobID`.
- Shift assignments tracked via `JobShiftID`.
- Manages scheduling using `Dates`.

### 2. Bonus Calculation
- `BonusDefault`: Default rate.
- `BonusPaid`, `BonusPaidAmount`: Actual bonus details.
- `IsFixedBonusPaidAmount`: Supports fixed/calc-based bonuses.
- Stores bonus comments for reference.

### 3. CRUD Operations
- `SelectAll`, `SelectOne`
- `Insert`, `Update`, `Delete`
- `Exists`: Check if user is already assigned

### 4. Offline Support
- `SelectAllOffline`
- `InsertOffline`
- `Save`: Han
