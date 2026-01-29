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
