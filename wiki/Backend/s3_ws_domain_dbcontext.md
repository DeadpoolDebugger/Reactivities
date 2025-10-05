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
**Notes:**
    - **Id** is used as the primary key to ensure uniqueness accross activities.
    - **required** is used to avoid `null` reference issues.
    - This model will later be mapped to a database table using Entity Framework Core inside the Persistence project.