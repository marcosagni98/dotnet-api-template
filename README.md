# dotnet API template

## Project Overview

This project is a Clean Architecture template for ASP.NET, designed to provide a robust and maintainable solution architecture.

## Architecture Structure

The solution is organized into four primary projects:

- **Domain Project**: Contains core domain entities, value objects, and domain-specific logic
- **Application Project**: Implements business logic, DTOs, interfaces, and application services
- **Infrastructure Project**: Manages external concerns like database access, external service integrations
- **API Project**: Hosts minimal APIs, controllers, and application entry point

## Technologies and Patterns

### Key Technologies
- ASP.NET
- Entity Framework Core
- AutoMapper
- Minimal APIs
- FluentResult and FluentValidations
- Repository Pattern

### Design Principles
- Clean Architecture
- Dependency Inversion
- Separation of Concerns

## Project Setup

### Prerequisites
- .NET 8 SDK
- Visual Studio 2022 or JetBrains Rider
- SQL Server or PostgreSQL

### Installation Steps

1. Clone the repository
```bash
git git@github.com:marcosagni98/dotnet-api-template.git
```

2. Navigate to the project directory
```bash
cd dotnet-api-template
```

3. Restore NuGet packages
```bash
dotnet restore
```

4. Configure connection string in `appsettings.json`

5. Run database migrations
```bash
dotnet ef database update
```

6. Run the application
```bash
dotnet run
```

## Project Structure

```
├── src
│   ├── YourProject.API
│   │   ├── Endpoints
│   │   └── Program.cs
│   ├── YourProject.Application
│   │   ├── DTOs
│   │   ├── Interfaces
│   │   └── Services
│   ├── YourProject.Domain
│   │   ├── Entities
│   │   ├── Interfaces
│   │   └── ValueObjects
│   └── YourProject.Infrastructure
│       ├── Data
│       ├── Repositories
│       └── Services
```

## Minimal API Configuration Example

```csharp
public class UserEndpoints
{
    public static void MapUserEndpoints(this IEndpointRouteBuilder app)
    {
        app.MapGet("/users", async (IUserService userService) => 
        {
            var users = await userService.GetAllUsersAsync();
            return Results.Ok(users);
        })
        .Produces<IEnumerable<UserDto>>(200)
        .WithName("GetUsers")
        .WithTags("Users");

        app.MapPost("/users", async (CreateUserDto userDto, IUserService userService) => 
        {
            var createdUser = await userService.CreateUserAsync(userDto);
            return Results.CreatedAtRoute("GetUserById", new { id = createdUser.Id }, createdUser);
        })
        .Produces<UserDto>(201)
        .WithName("CreateUser")
        .WithTags("Users");
    }
}
```
## Key Configuration

### Dependency Injection

Configure services in `Program.cs`:

```csharp
builder.Services.AddScoped<IUserRepository, UserRepository>();
builder.Services.AddScoped<IUserService, UserService>();
builder.Services.AddAutoMapper(typeof(Program));

// Map endpoints
app.MapUserEndpoints();
```



### Repository Pattern Example

```csharp
public interface IUserRepository
{
    Task<User> GetByIdAsync(int id);
    Task<IEnumerable<User>> GetAllAsync();
    Task AddAsync(User user);
    Task UpdateAsync(User user);
    Task DeleteAsync(int id);
}
```

## Testing

- Unit Tests: Located in a separate test project
- Uses xUnit or NUnit
- Covers domain logic, repositories, and services

## Logging

Configured with:
- Microsoft.Extensions.Logging

## OpenAPI Support

OpenAPI is configured for API documentation and testing.


## Deployment

### Docker Support
- Dockerfile included
- Docker Compose configuration available


## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

Distributed under the MIT License. See `LICENSE` for more information.
