## Setting up EF Core Migrations and Database Seeding
Now that we have configured `AppDbContext` and the connection string, the next step is to create and apply **Entity Framework Core migrations**. We’ll also configure **database seeding** so that our application starts with some default `Activity` records.
##
### 1. Install EF Core CLI Tool
First, ensure that the EF Core CLI tool is installed:
```bash
cd API
dotnet tool install --global dotnet-ef
```
> :warning: If the tool is already installed, you can skip this step.
##
### 2. Create and Apply Migration
Run the following commands from the **solution root folder**:
```bash
dotnet ef migrations add InitialCreate -p Persistence -s API
```
- `-p Persistence` → Specifies the **Persistence project** where migrations will be stored.<br/>
- `-s API` → Specifies the **startup project (API)** which provides configuration and dependency injection. <br/>

This will generate a **Migrations** folder inside the **Persistence** project.

Next, update the database:
```bash
dotnet ef database update -p Persistence -s API
```
This will create a **reactivities.db** SQLite database file inside the API project folder.

**Drop the database for Seeding**
Since we need to seed data during the initial migration, drop the database first:
```bash
dotnet ef database drop -p Persistence -s API
```
This ensures a clean slate for seeding data.
##
### 3. Create a Data Seeder
Inside the **Persistence** project, create a new folder & file `SeedData/DbInitializer.cs`:

File: `SeedData/DbInitializer.cs`
```csharp
using Domain;
using Microsoft.EntityFrameworkCore;

namespace Persistence
{
    public class DbInitializer
    {
        public static async Task SeedData(AppDbContext context)
        {
            // Exit if data already exists
            if (context.Activities.Any()) return;

            var activities = new List<Activity>
            {
                // Replace with actual seed data snippet eg
                //new() {
                //Title = "Past Activity 1",
                //Date = DateTime.Now.AddMonths(-2),
                //Description = "Activity 2 months ago",
                //Category = "drinks",
                //City = "London",
                //Venue = "The Lamb and Flag, 33, Rose Street, Seven Dials, Covent Garden, //London, Greater London, England, WC2E 9EB, United Kingdom",
                //Latitude = "51.51171665",
                //Longitude = "-0.1256611057818921",
                //}
            };

            context.Activities.AddRange(activities);
            await context.SaveChangesAsync();
        }
    }
}
```
##
### 4. Run Migration and Seeding at Startup
Modify **Program.cs** in the **API** project to ensure the database is migrated and seeded when the application starts:
File: `API/Program.cs`
```csharp
app.MapControllers();

// Apply migrations and seed data
using var scope = app.Services.CreateScope();
var services = scope.ServiceProvider;

try
{
    var context = services.GetRequiredService<AppDbContext>();
    await context.Database.MigrateAsync();
    await DbInitializer.SeedData(context);
}
catch (Exception ex)
{
    var logger = services.GetRequiredService<ILogger<Program>>();
    logger.LogError(ex, "An error occurred during migration or seeding.");
}

app.Run();
```
##
### 6. Run the Application
Now, run the application using:
```bash
dotnet watch
```
- EF Core will create/update the database.<br/>
- The seeding logic will populate the Activities table with initial data.<br/>
- The API is now ready to serve requests with seeded records.<br/>
##

:white_check_mark: At this point, we have:<br/>
- Created and applied EF migrations.<br/>
- Configured automatic database migration and seeding at startup.<br/>
- Ensured that the application runs with default Activity records.<br/>
