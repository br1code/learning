# Middlewares

Middleware in ASP.NET Core is a way to handle and process incoming HTTP requests and outgoing HTTP responses in the application's request pipeline. Middleware components can be thought of as a chain of responsibility, where each middleware has the opportunity to execute code before and/or after the next middleware in the pipeline. This allows middleware components to manipulate requests, responses, or control the flow of execution.

## Middleware Components

Middleware components are classes that implement specific functionality, such as authentication, logging, routing, error handling, or serving static files. Each middleware component must have an `Invoke` or `InvokeAsync` method that takes an `HttpContext` object as its parameter. The method is responsible for calling the next middleware in the pipeline using a delegate, typically called `_next`.

Here's a simple example of a custom middleware that logs the time taken to process a request:

```csharp
public class RequestTimingMiddleware
{
    private readonly RequestDelegate _next;

    public RequestTimingMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        var watch = new Stopwatch();
        watch.Start();

        // Call the next middleware in the pipeline
        await _next(context);

        watch.Stop();
        var elapsedTime = watch.ElapsedMilliseconds;

        // Log the time taken to process the request
        context.Response.Headers.Add("X-Elapsed-Time", elapsedTime.ToString());
    }
}
```

## Middleware Registration

Middleware components must be registered in the `Configure` method of the `Startup` class to be part of the request pipeline. Middleware registration order is important, as it determines the order in which they will be executed. You can register middleware using the `Use` or `UseMiddleware` extension methods on the `IApplicationBuilder` instance.

Here's an example of registering the custom `RequestTimingMiddleware` in the `Startup` class:

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        // Register the custom middleware
        app.UseMiddleware<RequestTimingMiddleware>();

        // Register other middlewares
        app.UseRouting();
        app.UseAuthentication();
        app.UseAuthorization();
        app.UseEndpoints(endpoints =>
        {
            endpoints.MapControllers();
        });
    }
}
```

You can achieve the same in a simpler way:

```C#
app.Use(async (context, next) =>
{
    var watch = new Stopwatch();
    watch.Start();

    // Call the next middleware in the pipeline
    await _next(context);

    watch.Stop();
    var elapsedTime = watch.ElapsedMilliseconds;

    // Log the time taken to process the request
    context.Response.Headers.Add("X-Elapsed-Time", elapsedTime.ToString());
});
```

## Built-in Middleware Components

ASP.NET Core provides several built-in middleware components, such as:

1. **Routing Middleware:** Handles request routing to the appropriate controller and action method based on route templates and constraints.

```csharp
app.UseRouting();
```

2. **Authentication Middleware:** Handles user authentication by validating credentials and issuing authentication tokens.

```csharp
app.UseAuthentication();
```

3. **Authorization Middleware:** Handles user authorization by enforcing role or policy-based access control.

```csharp
app.UseAuthorization();
```

4. **Static Files Middleware:** Serves static files, such as JavaScript, CSS, and images, from the file system.

```csharp
app.UseStaticFiles();
```

5. **Exception Handling Middleware:** Catches unhandled exceptions and generates error pages or custom error responses.

```csharp
app.UseExceptionHandler("/Error");
```

6. **CORS Middleware:** Handles Cross-Origin Resource Sharing (CORS) policy enforcement, allowing or denying requests from different origins.

```csharp
app.UseCors("AllowSpecificOrigins");
```

## Common use cases and scenarios

Here are five common use cases for middlewares in ASP.NET Core web applications and Web APIs, along with sample code for each:

1. **Logging:** Middleware can be used to log request and response details, such as HTTP method, URI, status code, and execution time.

```csharp
public class LoggingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<LoggingMiddleware> _logger;

    public LoggingMiddleware(RequestDelegate next, ILogger<LoggingMiddleware> logger)
    {
        _next = next;
        _logger = logger;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        var request = context.Request;
        _logger.LogInformation($"Request: {request.Method} {request.Path}");

        await _next(context);

        var response = context.Response;
        _logger.LogInformation($"Response: {response.StatusCode}");
    }
}
```

2. **Exception Handling:** Middleware can be used to catch unhandled exceptions and provide a custom error response or error page.

```csharp
public class ExceptionHandlingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<ExceptionHandlingMiddleware> _logger;

    public ExceptionHandlingMiddleware(RequestDelegate next, ILogger<ExceptionHandlingMiddleware> logger)
    {
        _next = next;
        _logger = logger;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        try
        {
            await _next(context);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "An unhandled exception occurred");
            context.Response.StatusCode = 500;
            await context.Response.WriteAsync("An error occurred. Please try again later.");
        }
    }
}
```

3. **Caching:** Middleware can be used to implement caching strategies, such as setting cache headers for specific resources.

```csharp
public class CacheControlMiddleware
{
    private readonly RequestDelegate _next;

    public CacheControlMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        var response = context.Response;

        // Set cache headers for static files
        if (context.Request.Path.StartsWithSegments("/static"))
        {
            response.Headers["Cache-Control"] = "public, max-age=86400";
        }

        await _next(context);
    }
}
```

4. **API Rate Limiting:** Middleware can be used to limit the rate at which clients can make requests to your API, protecting it from abuse.

```csharp
// This example uses the AspNetCoreRateLimit package (https://github.com/stefanprodan/AspNetCoreRateLimit)
public void ConfigureServices(IServiceCollection services)
{
    // ...
    services.AddMemoryCache();
    services.Configure<IpRateLimitOptions>(Configuration.GetSection("IpRateLimiting"));
    services.Configure<IpRateLimitPolicies>(Configuration.GetSection("IpRateLimitPolicies"));
    services.AddSingleton<IIpPolicyStore, MemoryCacheIpPolicyStore>();
    services.AddSingleton<IRateLimitCounterStore, MemoryCacheRateLimitCounterStore>();
    services.AddSingleton<IRateLimitConfiguration, RateLimitConfiguration>();
    // ...
}

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    // ...
    app.UseIpRateLimiting();
    // ...
}
```

5. **API Versioning:** Middleware can be used to handle API versioning by examining the request and routing it to the appropriate version of your API.

```csharp
// This example uses the Microsoft.AspNetCore.Mvc.Versioning package
public void ConfigureServices(IServiceCollection services)
{
    // ...
    services.AddApiVersioning(options =>
    {
        options.ReportApiVersions = true;
        options.AssumeDefaultVersionWhenUnspecified = true;
        options.DefaultApiVersion = new ApiVersion(1;
        options.ApiVersionReader = new HeaderApiVersionReader("api-version");
    });
    // ...
}

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    // ...
    app.UseRouting();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
    // ...
}
```

This example demonstrates how to use the `Microsoft.AspNetCore.Mvc.Versioning` package to handle API versioning. It configures the API to use a default version (1.0) when no version is specified and reads the version number from the "api-version" header in the incoming request. Clients can request a specific version of the API by including the "api-version" header with the desired version number in their requests.

## Middlewares vs Filters

Filters and middlewares are both mechanisms used in ASP.NET Core to handle and modify HTTP requests and responses. However, they have different purposes, scopes, and are used at different stages of the request pipeline. Here's a simple comparison to help you understand the differences between filters and middlewares:

**Middlewares**

1. Purpose: Middlewares are more general-purpose and can be used in any ASP.NET Core application, including MVC, Razor Pages, Web API, and others. They allow you to handle and modify incoming HTTP requests and outgoing HTTP responses, acting as a chain of components in the request pipeline.

2. Scope: Middlewares are application-wide, meaning they are applied to every request coming into the application and every response going out.

3. Execution Order: Middlewares are executed in the order they are added to the pipeline, with the request flowing through each middleware component in sequence, and the response flowing back through the same sequence in reverse.

4. Context: Middlewares have access to the HttpContext, which includes the request and response objects, but they do not have direct access to action-specific context or detailed information about MVC or Razor Pages components.

5. Use Cases: Middlewares are commonly used for tasks like request/response logging, routing, authentication, CORS, compression, exception handling, and more.

**Filters**

1. Purpose: Filters are specifically designed for ASP.NET Core MVC and Razor Pages applications. They allow you to perform pre-processing and post-processing of action methods, result execution, exception handling, and other parts of the request pipeline that are specific to MVC and Razor Pages.

2. Scope: Filters are scoped to controllers and action methods, meaning they can be applied to individual controllers or actions to modify their behavior.

3. Execution Order: Filters are executed in a specific order based on their type: Authorization filters, Resource filters, Action filters, Result filters, and Exception filters.

4. Context: Filters have access to the context of the action or the result being executed, providing more detailed information about the current request, such as action arguments, ModelState, ViewData, TempData, and more.

5. Use Cases: Filters are commonly used for tasks like authorization, caching, logging, validation, exception handling, and modifying action results.

To summarize, filters are primarily used for tasks specific to MVC and Razor Pages applications, while middlewares are more general-purpose and can be used across different types of ASP.NET Core applications. Filters operate within the context of controllers and actions, while middlewares work at the application level, processing all incoming requests and outgoing responses.

## Conclusion

In summary, middleware in ASP.NET Core is a powerful way to handle and process HTTP requests and responses in your application's request pipeline. Middleware components can be chained together to implement various application features, such as authentication, logging, routing, and more. By registering middleware in the `Configure` method of the `Startup` class, you can control the order in which middleware components are executed and customize your application's behavior
