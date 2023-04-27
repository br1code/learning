# Error Handling

Source: https://learn.microsoft.com/en-us/aspnet/core/fundamentals/error-handling?view=aspnetcore-7.0

Error handling in modern ASP.NET Core is an essential aspect of building robust and user-friendly web applications and APIs. ASP.NET Core provides several mechanisms for handling errors gracefully and consistently. Here's an in-depth explanation of error handling in ASP.NET Core, including code examples.

**1. Developer Exception Page:**
The Developer Exception Page is a built-in middleware that shows detailed error information when an unhandled exception occurs in the application during development. To enable the Developer Exception Page, add the following code in the `Configure` method of the `Startup.cs` file:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    // Other middlewares
    app.UseRouting();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

The Developer Exception Page should only be used in the development environment, as it can expose sensitive information in a production environment.

**2. Custom Error Pages:**
In a production environment, you'll want to display user-friendly error pages instead of exposing exception details. You can use the `UseStatusCodePages` or `UseStatusCodePagesWithRedirects` middleware to handle non-success status codes (e.g., 404, 500) and display custom error pages. Add the following code in the `Configure` method of the `Startup.cs` file:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseStatusCodePagesWithRedirects("/Error/{0}");
    }

    // Other middlewares
    app.UseRouting();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

Create an `ErrorController` with an action method that accepts the status code as a parameter:

```csharp
public class ErrorController : Controller
{
    [Route("Error/{statusCode}")]
    public IActionResult Index(int statusCode)
    {
        return View(statusCode);
    }
}
```

Now, when an error occurs, the user will be redirected to the custom error page.

**3. Exception Handling Middleware:**
For handling exceptions globally in your application, you can use the `UseExceptionHandler` middleware. This middleware allows you to capture exceptions, log them, and return a custom error response. Add the following code in the `Configure` method of the `Startup.cs` file:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error/HandleException");
        app.UseStatusCodePagesWithRedirects("/Error/{0}");
    }

    // Other middlewares
    app.UseRouting();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

Create an action method in the `ErrorController` to handle exceptions:

```csharp
public class ErrorController : Controller
{
    [Route("Error/HandleException")]
    public IActionResult HandleException()
    {
        var exceptionHandlerPathFeature = HttpContext.Features.Get<IExceptionHandlerPathFeature>();
        var exception = exceptionHandlerPathFeature?.Error;

        // Log the exception here

        return View("Error");
    }
}
```

This way, when an exception occurs, it will be handled by the `HandleException` action method, which can log the exception and display a custom error page.

**4. Exception Filters:**
Exception filters allow you to handle exceptions that occur within MVC actions. You can create a custom exception filter by implementing the `IExceptionFilter` interface or deriving from the `ExceptionFilterAttribute` class. The custom exception filter can handle exceptions, log them, and return a custom error response. Here's an example of a custom exception filter:

```csharp
public class CustomExceptionFilterAttribute : ExceptionFilterAttribute
{
    public override void OnException(ExceptionContext context)
    {
        var exception = context.Exception;

        // Log the exception here

        context.Result = new JsonResult(new { error = "An error occurred." });
        context.ExceptionHandled = true;
    }
}
```

To apply the exception filter globally, add it to the `MvcOptions` in the `ConfigureServices` method of the `Startup.cs` file:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers(options =>
    {
        options.Filters.Add(new CustomExceptionFilterAttribute());
    });
}
```

Alternatively, you can apply the exception filter to specific controllers or action methods using the attribute:

```csharp
[CustomExceptionFilter]
public class HomeController : Controller
{
    // Action methods
}
```

With this custom exception filter in place, any exceptions thrown within the MVC actions will be handled by the `OnException` method, logged, and a custom error response will be returned.

In conclusion, ASP.NET Core provides multiple mechanisms for handling errors, allowing you to create a consistent and user-friendly experience. By using a combination of Developer Exception Page, custom error pages, exception handling middleware, and exception filters, you can effectively manage errors in both development and production environments, as well as log exceptions for further analysis and debugging.