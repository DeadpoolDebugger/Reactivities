### 1: Create Solution and Project Structure
Open a terminal and run the following commands:

```bash
# Create solution folder
mkdir Reactivities && cd Reactivities

# Create solution file (name auto-taken from folder)
dotnet new sln  

# Create Projects
dotnet new webapi -n API
dotnet new classlib -n Domain
dotnet new classlib -n Application
dotnet new classlib -n Persistence
```
##
### 2: Add Projects to Solution
```bash
# Add projects to solution
dotnet sln add API/API.csproj
dotnet sln add Domain/Domain.csproj
dotnet sln add Application/Application.csproj
dotnet sln add Persistence/Persistence.csproj
```
##
### 3: Define Project References
We need to link the projects in the correct order to follow **Clean Architecture**:

- **API** → depends on **Application**
- **Application** → depends on **Domain** and **Persistence**
- **Persistence** → depends on **Domain**

```bash
# Add references
dotnet add API/API.csproj reference Application/Application.csproj
dotnet add Application/Application.csproj reference Domain/Domain.csproj
dotnet add Application/Application.csproj reference Persistence/Persistence.csproj
dotnet add Persistence/Persistence.csproj reference Domain/Domain.csproj
```
##
[<< Back to Backend Main Page](https://github.com/DeadpoolDebugger/Reactivities/blob/main/wiki/Backend/BACKEND.md)
