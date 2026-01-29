# RiseFSM: Field Service Management System - Complete Guide

This is a comprehensive guide combining business and technical aspects of the RiseFSM system.

# RiseFSM Business Guide

## Overview
RiseFSM is a Field Service Management system for oil & gas service companies.

## Core Business Entities

### Cost Items
- Master service catalog
- Defines what can be charged to customers
- Classifications: Tool, Person, Fleet, General

### Price Items  
- Customer-specific pricing
- Contract rates override base pricing
- Regional and zone-based pricing

### Jobs
- Central work orders
- Status lifecycle: Quote ? Pending ? Active ? Complete ? Invoiced
- Contains all job-related data

### Tickets
- Billable line items
- Types: Standard, Bid Items (require approval), Additional Items
- Links to equipment and personnel usage

### Load Out
- Equipment deployment tracking
- Links physical assets to billing
- Usage tracking for asset utilization

### Quotes
- Customer proposals and estimates
- Multiple versions per opportunity
- Convert to jobs when approved

## Business Workflows

### Sales Process
1. Lead Management
2. Quote Development
3. Contract Negotiation
4. Award

### Operations Process
1. Job Planning
2. Resource Allocation
3. Equipment Deployment
4. Field Operations
5. Documentation

### Financial Process
1. Time & Material Tracking
2. Billing
3. Invoice Generation
4. Payment Collection

## Key Business Rules

### Pricing Hierarchy
Cost Items ? Price Items ? Tickets
Base catalog ? Customer rates ? Actual billing

### Approval Workflows
- Standard tickets: Auto-approved
- Bid items: Require management approval
- Additional items: Emergency work tracking

### Equipment Integration
Load Out deployment automatically creates billing opportunities
Physical asset usage tracked and converted to revenue

## Industry Context

### Service Types
- Well Intervention: Remedial well work
- Wireline: Logging and precision operations  
- Completion: Well completion and fracturing

### Challenges
- Remote locations with poor connectivity
- High-value equipment ($500K - $20M per unit)
- Safety-critical operations
- Complex contracts and pricing

### Business Model
- Contract-based pricing
- Equipment utilization focus
- Expertise premium pricing
- Risk management critical


# RiseFSM Developer Guide

## Technology Stack

### Backend
- .NET 6 Razor Pages
- Entity Framework Core
- SQL Server
- Multi-tenant architecture
- Domain-driven design

### Frontend  
- Vue.js 2
- TypeScript
- Bootstrap 4
- DevExpress components

### Data
- SQL Server (primary)
- IndexedDB (offline storage)
- Azure Blob Storage (files)

## Architecture Patterns

### Multi-Tenant Design
```csharp
public class DataModelBase 
{
    protected readonly int _tenantID;
    
    // All queries include tenant isolation
    protected string BuildQuery(string baseQuery) 
    {
        return $"{baseQuery} AND TenantID = {_tenantID}";
    }
}
```

### Offline-First Pattern
```javascript
// Save locally first, sync later
async saveData(data) {
    await saveToIndexedDB(data);
    await queueForSync(data);
    if (online) await syncToServer(data);
}
```

### Domain-Driven Design
```
Web/
??? Domain/           # Business logic
??? Models/Public/    # Data transfer objects  
??? API/              # REST endpoints
??? Pages/            # Razor Pages UI
??? Services/         # Application services
```

## Common Development Patterns

### Creating New API Endpoint
```csharp
[Route("api/myentity")]
public class MyEntityController : APIControllerBase
{
    [HttpGet]
    public IActionResult Get()
    {
        var domain = new Domain.MyEntity(ConnectionString, TenantID, UserID);
        return Ok(domain.SelectAll());
    }
    
    [HttpPost]
    public IActionResult Post([FromBody] MyEntity model)
    {
        var domain = new Domain.MyEntity(ConnectionString, TenantID, UserID);
        var errors = domain.Insert(model);
        return errors.Any() ? BadRequest(errors) : Ok(model);
    }
}
```

### Essential Files

#### Configuration
- `Domain/Utility/Const.cs` - Constants and enums
- `Domain/CodeTableEnumsPartial.cs` - Configuration tables
- `appsettings.json` - Application settings

#### Core Business
- `Domain/Job.cs` - Job management
- `Domain/Ticket.cs` - Billing logic
- `Domain/CostItem.cs` - Service catalog
- `Domain/Asset.cs` - Equipment management

#### Key APIs
- `API/Jobs/JobController.cs` - Job operations
- `API/Jobs/TicketController.cs` - Billing operations
- `API/Jobs/LoadOutController.cs` - Equipment operations


## Advanced Topics

### Multi-Tenant Security
Every data operation includes automatic tenant filtering to ensure complete data isolation between companies.

### Offline Capabilities
The system operates seamlessly offline with local data storage and background synchronization.

### Business Intelligence
Built-in reporting and analytics provide insights into operations, profitability, and performance.
