# REACTIVITIES - Backend Documentation
The backend of the Reactivities application is developed using **.Net Core 9(C#)**. We follow a **Clean Architecture** approach, separating responsibilities into different projects for maintainability and scalability.


We are using:
- Entity Framework Core (EF Core) as the ORM
- SQLite as the temporary database(only for development purpose)

## Project Structure
The solution is organized into four projects:

1. API (Web API project)