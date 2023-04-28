# HealthChecks

Health checks are a way to determine the health of your ASP.NET Core application and its dependencies, such as databases, external services, or caches. They allow you to monitor the status of your application and its components, enabling you to proactively identify issues, perform maintenance tasks, or redirect traffic away from unhealthy instances.

By implementing health checks, you can improve the reliability and stability of your application, ensuring that it remains available and performant even when some of its components are experiencing issues.

## Why use health checks

Health checks can be used for:

1. Monitoring the health of your application and its dependencies in real-time.
2. Load balancers and orchestrators, such as Kubernetes, can use health checks to determine if an instance is healthy and can receive traffic or needs to be restarted.
3. Providing a consistent way to check the health of multiple services in a microservices architecture.
4. Implementing custom logic to determine the health of specific components, such as caches or external APIs.

## How to use health checks in ASP.NET Core

To implement health checks in an ASP.NET Core application, follow these steps:

1. **Install the necessary NuGet packages:**

   First, you need to install the `Microsoft.Extensions.Diagnostics.HealthChecks` and `Microsoft.AspNetCore.Diagnostics.HealthChecks` NuGet packages. You can do this using the Package Manager Console, the .NET CLI, or by editing your project file directly.

   Using the .NET CLI:

   ```
   dotnet add package Microsoft.Extensions.Diagnostics.HealthChecks
   dotnet add package Microsoft.AspNetCore.Diagnostics.HealthChecks
   ```

2. **Configure health checks in the Startup class:**

   In the `Startup.cs` file, update the `ConfigureServices` method to add health checks:

   ```csharp
   public void ConfigureServices(IServiceCollection services)
   {
       services.AddHealthChecks()
           .AddCheck("Example", () => 
               HealthCheckResult.Healthy("The example health check is healthy"), 
               tags: new[] { "example" });

       // Add other services, such as MVC or Razor Pages
       services.AddControllersWithViews();
   }
   ```

   In the example above, a custom health check named "Example" is added. It always returns a healthy status. You can replace this with your own logic or add more health checks for different components.

3. **Expose health check endpoints:**

   In the `Startup.cs` file, update the `Configure` method to map health check endpoints:

   ```csharp
   public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
   {
       // ... Other middleware configurations, such as UseExceptionHandler or UseStaticFiles

       app.UseRouting();

       app.UseEndpoints(endpoints =>
       {
           endpoints.MapHealthChecks("/health");
           endpoints.MapHealthChecks("/health/example", new HealthCheckOptions
           {
               Predicate = (check) => check.Tags.Contains("example"),
               ResponseWriter = WriteResponse
           });

           // Other endpoint mappings, such as MapControllerRoute
           endpoints.MapControllerRoute(
               name: "default",
               pattern: "{controller=Home}/{action=Index}/{id?}");
       });
   }
   
   private static Task WriteResponse(HttpContext context, HealthReport result)
   {
       context.Response.ContentType = "application/json";
       var json = JsonSerializer.Serialize(new
       {
           status = result.Status.ToString(),
           errors = result.Entries.Select(e => new { key = e.Key, value = Enum.GetName(typeof(HealthStatus), e.Value.Status) })
       });
       return context.Response.WriteAsync(json);
   }
   ```

   When you use `app.MapHealthChecks`, you specify a URL path and optionally a configuration object (HealthCheckOptions). The middleware will then listen to the specified URL path, and when a request is made to that path, it will execute the registered health checks and return the results.

   In the example above, two health check endpoints are mapped: one at `/health` that includes all health checks, and one at `/health/example` that includes only healthchecks with the "example" tag. The `/health/example` endpoint also uses a custom response writer to format the health check results as JSON.

4. **Implement custom health checks:**

   You can create custom health checks by implementing the `IHealthCheck` interface. For example, to create a health check for a database connection:

   ```csharp
   using System;
   using System.Data.SqlClient;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public class DatabaseHealthCheck : IHealthCheck
   {
       private readonly string _connectionString;

       public DatabaseHealthCheck(string connectionString)
       {
           _connectionString = connectionString;
       }

       public async Task<HealthCheckResult> CheckHealthAsync(HealthCheckContext context, CancellationToken cancellationToken = default)
       {
           try
           {
               using (var connection = new SqlConnection(_connectionString))
               {
                   await connection.OpenAsync(cancellationToken);
                   return HealthCheckResult.Healthy("Database connection is healthy");
               }
           }
           catch (Exception ex)
           {
               return HealthCheckResult.Unhealthy("Database connection is unhealthy", ex);
           }
       }
   }
   ```

   To register the custom health check, update the `ConfigureServices` method in the `Startup.cs` file:

   ```csharp
   public void ConfigureServices(IServiceCollection services)
   {
       services.AddHealthChecks()
           .AddCheck<DatabaseHealthCheck>("Database", tags: new[] { "database" });

       // ... Other service configurations
   }
   ```

## Common use cases

1. Monitoring the health of a database connection: You can create a custom health check that tests the connection to your database, as shown in the example above.
2. Monitoring the health of external services: You can create health checks that test the availability and response times of external services, such as REST APIs or messaging systems.
3. Monitoring the health of a cache: You can create health checks that test the functionality of caching systems, such as Redis or in-memory caches.
4. Monitoring custom application metrics: You can create health checks that monitor specific application metrics, such as queue lengths, error rates, or response times.

By implementing health checks in your ASP.NET Core application, you can improve its reliability and stability, ensuring it remains available and performant even when some of its components are experiencing issues.

## Adding a HealthCheck vs Mapping a HealthCheck: In depth

**Adding a health check:**

Adding a health check refers to the process of registering one or more health checks in your application's dependency injection container. This is done using the `services.AddHealthChecks()` method in the `ConfigureServices` method of your `Startup.cs` file.

When you add a health check, you configure the health check's settings, such as the implementation of the health check (e.g., a custom health check for a database connection) and any options like tags, custom response writers, or failure statuses.

Here's an example of adding a health check:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck<DatabaseHealthCheck>("Database", tags: new[] { "database" });

    // ... Other service configurations
}
```

In the example above, a custom `DatabaseHealthCheck` is added and registered in the dependency injection container with the name "Database" and a tag "database".

**Mapping a health check:**

Mapping a health check refers to the process of associating one or more health checks with specific URL paths in your application. This is done using the `app.MapHealthChecks()` method in the `Configure` method of your `Startup.cs` file.

When you map a health check, you create a new endpoint in your application that listens to a specific URL path. When a request is made to that URL path, the health check middleware executes the registered health checks associated with that endpoint and returns the results.

Here's an example of mapping a health check:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    // ... Other middleware configurations, such as UseExceptionHandler or UseStaticFiles

    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHealthChecks("/health");
        endpoints.MapHealthChecks("/health/database", new HealthCheckOptions
        {
            Predicate = (check) => check.Tags.Contains("database"),
            ResponseWriter = WriteResponse
        });

        // Other endpoint mappings, such as MapControllerRoute
        endpoints.MapControllerRoute(
            name: "default",
            pattern: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

In the example above, two health check endpoints are mapped: one at `/health` that includes all health checks, and one at `/health/database` that includes only health checks with the "database" tag.

**Summary:**

- Adding a health check: Registering and configuring health checks in the dependency injection container using `services.AddHealthChecks()`.
- Mapping a health check: Associating health checks with specific URL paths in your application using `app.MapHealthChecks()`.

You need to perform both adding and mapping health checks in your ASP.NET Core application to have a complete health check setup.