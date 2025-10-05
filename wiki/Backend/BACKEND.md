# REACTIVITIES - Backend Documentation
The backend of the Reactivities application is developed using **.Net Core 9(C#)**. We follow a **Clean Architecture** approach, separating responsibilities into different projects for maintainability and scalability.


We are using:
- Entity Framework Core (EF Core) as the ORM
- SQLite as the temporary database(only for development purpose)

## :open_file_folder: Project Structure
The solution is organized into four projects:

1. **API (Web API project)**
   - Entry point of the backend application, responsible for handling HTTP requests and returning responses
   
2. **Domain (Class Library)**
   - Defines the core entities(models) of the application.

3. **Application (Class Library)**
   - Contains the business rules and logic
  
4. **Persistence (Class Library)**
   - Contains DbContext, Migrations and EF Core configs, Repositories
  
## :rocket: Getting Started

### Step 1: API Walking Skeleton
   > [Create Solution and Project Structure](https://github.com/DeadpoolDebugger/Reactivities/blob/main/wiki/Backend/s1_ws_project_solution.md)<br/>
   > [Backend Cleanup and Simplification](https://github.com/DeadpoolDebugger/Reactivities/blob/main/wiki/Backend/s2_ws_cleanup.md)
   > [Domain and DbContext](https://github.com/DeadpoolDebugger/Reactivities/blob/main/wiki/Backend/s3_ws_domain_dbcontext.md)
