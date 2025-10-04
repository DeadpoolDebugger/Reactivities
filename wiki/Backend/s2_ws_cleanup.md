## Backend Cleanup and Simplification
Before diving deeper, we will simplify our backend setup by removing unnecessary files and services. This will make our application cleaner and easier to understand.

### 1. Update `launchSettings.json`

In the **API** project, go to `Properties/launchSettings.json` and update it to use only **HTTPS** on port **5001**.

```json
{
  "$schema": "https://json.schemastore.org/launchsettings.json",
  "profiles": {
    "https": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": false,
      "applicationUrl": "https://localhost:5001",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}

```

**Troubleshooting HTTPS Certificates**

If you face issues running on https://localhost:5001, reset your development certificates:
```bash
cd API
dotnet dev-certs https --clean   # remove existing certificates
dotnet dev-certs https --trust   # generate and trust a new certificate
dotnet dev-certs https --check   # verify the certificate
```
___
### 2. Remove OpenAPI/Swagger Dependencies

In API.csproj, delete the following section:
```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.OpenApi" Version="10.0.0-rc.1.25451.107" />
</ItemGroup>
```
Also, delete the **Api.http** file (not required for this application).
___
### 3. Clean Up Program.cs

Remove all unnecessary OpenAPI, HTTPS redirection, and authorization services. Your Program.cs should now look like this:

```csharp
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddControllers();

var app = builder.Build();
app.MapControllers();
app.Run();
```
___

### :white_check_mark: Final Result

- No extra Swagger/OpenAPI dependencies.
- HTTPS-only configuration on localhost:5001.
- Clean Program.cs with just controllers and minimal setup.

This ensures our backend is lightweight, simple, and easy to extend as we add features later.

