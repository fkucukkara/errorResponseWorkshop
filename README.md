# Error Response Handling in Minimal APIs

This repository demonstrates an implementation of **error response handling** in .NET Minimal APIs using `ProblemDetails`. It showcases how to handle exceptions and status codes in a structured and user-friendly way while adhering to best practices.

## Features
- **Standardized Error Responses**: Uses `ProblemDetails` to generate error responses conforming to the [RFC 7807](https://datatracker.ietf.org/doc/html/rfc7807) standard.
- **Automatic Error Handling**: Simplifies the handling of exceptions and HTTP status codes with middleware integration.
- **Minimal API Integration**: Lightweight and concise implementation for modern .NET applications.

## Advantages
1. **Improved API Consumer Experience**:
   - Provides consistent and meaningful error messages to API clients.
   - Helps consumers of the API understand what went wrong and how to resolve issues.

2. **Standardized Format**:
   - The use of `ProblemDetails` ensures that error responses follow a predictable structure, which is crucial for debugging and integration with client applications.

3. **Reduced Boilerplate**:
   - The integration of `ProblemDetails`, exception handling, and status code pages reduces the need for custom error-handling code.

4. **Extensibility**:
   - Easily customizable to include additional error details, such as trace IDs, request URLs, or custom metadata.

## Implementation
The implementation uses Minimal APIs and middleware to handle exceptions and status codes.

### Code Example
```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddProblemDetails();

var app = builder.Build();

app.UseExceptionHandler();
app.UseStatusCodePages();

app.MapGet("/users/{id:int}", (int id)
    => id <= 0 ? Results.BadRequest() : Results.Ok(new User(id)));

app.Run();

internal record User(int Id);
```

### Explanation
1. **`AddProblemDetails`**:
   - Registers the `ProblemDetails` middleware, which generates standardized error responses.

2. **`UseExceptionHandler`**:
   - Handles unhandled exceptions and generates error responses.

3. **`UseStatusCodePages`**:
   - Adds support for returning proper responses for HTTP status codes (e.g., `400 Bad Request`, `404 Not Found`).

4. **Endpoint**:
   - Example endpoint `/users/{id:int}`:
     - Returns `400 Bad Request` if `id <= 0`.
     - Returns a `200 OK` response with a user object if `id > 0`.

## How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/fkucukkara/errorResponseWorkshop.git
   cd API
   ```

2. Build and run the application:
   ```bash
   dotnet run
   ```

3. Test the endpoints:
   - Valid request:
     ```
     GET /users/1
     Response: { "id": 1 }
     ```
   - Invalid request:
     ```
     GET /users/-1
     Response: HTTP 400 with ProblemDetails
     ```

## Example Error Response
For an invalid request, the API generates a response in the `ProblemDetails` format:

```json
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
  "title": "Bad Request",
  "status": 400,
  "detail": "The request parameters are invalid.",
  "instance": "/users/-1"
}
```

## License
[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

This project is licensed under the MIT License, which allows you to freely use, modify, and distribute the code. See the [`LICENSE`](LICENSE) file for full details.
