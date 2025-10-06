## Adding Controllers for Activities API
Now that we have seeded the database with `Activity` data, we need to expose it through an API.
We will first create a **Base API Controller** which all other controllers will inherit from. This helps maintain consistency and avoids repetitive attributes.
##

### 1. Create BaseApiController
Inside the **API/Controllers** folder, create a new file `BaseApiController.cs`:

File: `API/Controllers/BaseApiController.cs`
```csharp
using Microsoft.AspNetCore.Mvc;

namespace API.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class BaseApiController : ControllerBase
    {
    }
}
```
- `ControllerBase`→ Provides the basic functionality for API controllers (without views).
- `[ApiController]`→ Adds API-specific behaviors like automatic model validation and binding.
- `[Route("api/[controller]")]` → Sets the route pattern (e.g., `api/activities` for `ActivitiesController`).

All future controllers will inherit from this class instead of directly from ControllerBase.
##

### 2. Create ActivitiesController
Now, let’s create a controller for handling `Activity` data.<br/>
File: `API/Controllers/ActivitiesController.cs`
```csharp
using Domain;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using Persistence;

namespace API.Controllers
{
    public class ActivitiesController(AppDbContext context) : BaseApiController
    {
        // GET: api/activities
        [HttpGet]
        public async Task<ActionResult<List<Activity>>> GetActivities()
        {
            return await _context.Activities.ToListAsync();
        }

        // GET: api/activities/{id}
        [HttpGet("{id}")]
        public async Task<ActionResult<Activity>> GetActivity(string id)
        {
            var activity = await _context.Activities.FindAsync(id);

            if (activity == null)
                return NotFound();

            return activity;
        }
    }
}
```
Explanation
- `GetActivities()` → Returns a list of all activities.<br/>
- `GetActivity(id)` → Returns a single activity by its unique ID.<br/>
##

### 3. Run and Verify
Now run the project:
```bash
dotnet watch
```
Open the browser and test the following endpoints:
- `https://localhost:5001/api/activities` → Returns all seeded activities.
- `https://localhost:5001/api/activities/{id}` → Returns a specific activity by ID.
##

:white_check_mark: At this stage, we have:
- A **Base API Controller** to standardize routes.<br/>
- An **Activities Controller** with endpoints for retrieving data.<br/>
- Verified seeded activities from the database.<br/>
