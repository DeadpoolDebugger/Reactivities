## Model Creation
We will begin by creating our first model inside the Domain project.
The Activity model will serve as the main entity for this application.

File: `Domain/Activity.cs`
```csharp
namespace Domain;

public class Activity
{
    public string Id { get; set; } = Guid.NewGuid().ToString();
    public required string Title { get; set; }
    public DateTime Date { get; set; }
    public required string Description { get; set; }
    public required string Category { get; set; }
    public bool IsCancelled { get; set; }
    public required string City { get; set; }
    public required string Venue { get; set; }
    public required string Latitude { get; set; }
    public required string Longitude { get; set; }
}
```
**Notes:**<br/>
- `Id` is used as the primary key to ensure uniqueness accross activities.<br/>
- **required** is used to avoid `null` reference issues.<br/>
- This model will later be mapped to a database table using Entity Framework Core inside the        Persistence project.

___
## Setting up Entity Framework Core
After creating the **Activity** model in the **Domain** project, the next step is to configure **Entity Framework Core (EF Core)** for data access. This involves installing EF Core packages, creating a `DbContext class`, and registering it in the API project.

### 1. Install EF Core Packages
Run the following commands to install the required NuGet packages:
```bash
dotnet add Persistence package Microsoft.EntityFrameworkCore.Sqlite
dotnet add API package Microsoft.EntityFrameworkCore.Design
```
- **Microsoft.EntityFrameworkCore.Sqlite** → Provides EF Core support for SQLite database.
- **Microsoft.EntityFrameworkCore.Design** → Required for design-time services (e.g., migrations).
##
### 2. Create the `AppDbContext` Class
**File: `Persistence/AppDbContext.cs`**
```csharp
using Domain;
using Microsoft.EntityFrameworkCore;

namespace Persistence
{
    public class AppDbContext : DbContext
    {
        public AppDbContext(DbContextOptions options) : base(options)
        {
        }

        public DbSet<Activity> Activities { get; set; } = null!;
    }
}
```
**Explanation:**<br/>
- `AppDbContext` inherits **DbContext**, which is the primary class for working with EF Core.<br/>
- The constructor accepts `DbContextOptions` and passes it to the base class, allowing EF Core to configure the database provider (in this case, SQLite).<br/>
- `DbSet<Activity> Activities` represents a table in the database where each `Activity` object will map to a row.
##
### 3. Register `AppDbContext` in the API Project
In **API/Program.cs**, add the following code to register the `AppDbContext` service:
```csharp
builder.Services.AddDbContext<AppDbContext>(opt =>
{
    opt.UseSqlite(builder.Configuration.GetConnectionString("DefaultConnection"));
});
```
- `AddDbContext<AppDbContext>` registers the database context so it can be injected into controllers and services.<br/>
- `UseSqlite` tells EF Core to use SQLite as the database provider.<br/>
- The connection string is retrieved from **appsettings.Development.json**.
##
### 4. Add Connection String
In **API/appsettings.Development.json**, add the connection string:
```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Information"
    }
  },
  "ConnectionStrings": {
    "DefaultConnection": "Data source=reactivities.db"
  }
}
```

:white_check_mark: At this stage, the project is set up with EF Core and ready to run **migrations** to create the database schema.
