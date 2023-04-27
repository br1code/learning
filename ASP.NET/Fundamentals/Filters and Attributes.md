# Filters and Attributes

Filters and attributes in ASP.NET Core provide a way to execute custom code at different stages of the request pipeline. Filters are classes that can be applied to controllers, actions, or globally across the application, and they offer a way to introduce cross-cutting concerns without modifying the code of your controllers or actions directly.

There are several types of filters in ASP.NET Core:

1. Authorization filters: Implement the `IAuthorizationFilter` interface and are used to handle authentication and authorization logic.
2. Resource filters: Implement the `IResourceFilter` interface and can be used for short-circuiting the request pipeline or caching responses.
3. Action filters: Implement the `IActionFilter` or `IAsyncActionFilter` interface and are used to manipulate the input or output of an action, or to execute custom code before or after the action.
4. Result filters: Implement the `IResultFilter` or `IAsyncResultFilter` interface and are used to modify the action result or execute custom code before or after the result is executed.
5. Exception filters: Implement the `IExceptionFilter` or `IAsyncExceptionFilter` interface and are used to handle exceptions that occur during the request processing.

Attributes are a way to associate filters with controllers or actions. You can create custom attributes by inheriting from the appropriate filter base class or implementing the corresponding filter interface. Alternatively, you can use built-in attributes provided by ASP.NET Core.

Note: Filters are executed **in a specific order** based on their type: Authorization filters, Resource filters, Action filters, Result filters, and Exception filters.

Here are some examples of built-in attributes and custom filters:

## Authorization Filters

The `[Authorize]` attribute is a built-in authorization filter that restricts access to a controller or action to authenticated users.

```csharp
[Authorize]
public class AccountController : Controller
{
    // ...
}
```

## Resource Filters

The `[ResponseCache]` attribute is a built-in resource filter that controls the caching of an action's response.

```csharp
[ResponseCache(Duration = 60)]
public IActionResult CachedContent()
{
    return View();
}
```

## Action Filters

You can create a custom action filter by inheriting from the `ActionFilterAttribute` class.

```csharp
public class CustomActionFilter : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext context)
    {
        // Custom logic before the action executes.
    }

    public override void OnActionExecuted(ActionExecutedContext context)
    {
        // Custom logic after the action executes.
    }
}
```

Then apply the custom action filter to a controller or action:

```csharp
[CustomActionFilter]
public class HomeController : Controller
{
    // ...
}
```

## Result Filters

You can create a custom result filter by inheriting from the `ResultFilterAttribute` class.

```csharp
public class CustomResultFilter : ResultFilterAttribute
{
    public override void OnResultExecuting(ResultExecutingContext context)
    {
        // Custom logic before the result is executed.
    }

    public override void OnResultExecuted(ResultExecutedContext context)
    {
        // Custom logic after the result is executed.
    }
}
```

Then apply the custom result filter to a controller or action:

```csharp
[CustomResultFilter]
public class HomeController : Controller
{
    // ...
}
```

## Exception Filters

You can create a custom exception filter by implementing the `IExceptionFilter` interface.

```csharp
public class CustomExceptionFilter : IExceptionFilter
{
    public void OnException(ExceptionContext context)
    {
        // Custom logic to handle exceptions.
    }
}
```

Then apply the custom exception filter to a controller or action:

```csharp
[ServiceFilter(typeof(CustomExceptionFilter))]
public class HomeController : Controller
{
    // ...
}
```

**Note:** ASP.NET Core has the concept of "Endpoint Filters", but they are not a separate type of filter in ASP.NET Core like action or result filters. However, the concept of filtering and processing requests at the endpoint level can be achieved using ASP.NET Core **middleware and endpoint routing**.

## Common use cases and scenarios

Here are two examples for each type of filter in ASP.NET Core, covering common scenarios for web applications and web APIs.

**1. Authorization Filters**

Example 1: Restrict access to a controller or action for authenticated users only
```csharp
[Authorize]
public class AccountController : Controller
{
    // ...
}
```

Example 2: Restrict access to a specific role
```csharp
[Authorize(Roles = "Administrator")]
public class AdminController : Controller
{
    // ...
}
```

**2. Resource Filters**

Example 1: Cache the response for a specified duration
```csharp
[ResponseCache(Duration = 60)]
public IActionResult CachedContent()
{
    return View();
}
```

Example 2: Short-circuit the request pipeline if a specific condition is met
```csharp
public class ShortCircuitResourceFilter : Attribute, IResourceFilter
{
    public void OnResourceExecuting(ResourceExecutingContext context)
    {
        if (context.HttpContext.Request.Headers.ContainsKey("User-Agent"))
        {
            context.Result = new StatusCodeResult(StatusCodes.Status400BadRequest);
        }
    }

    public void OnResourceExecuted(ResourceExecutedContext context)
    {
    }
}

[ShortCircuitResourceFilter]
public class HomeController : Controller
{
    // ...
}
```

**3. Action Filters**

Example 1: Log the execution time of an action
```csharp
public class ExecutionTimeActionFilter : ActionFilterAttribute
{
    private Stopwatch _stopwatch;

    public override void OnActionExecuting(ActionExecutingContext context)
    {
        _stopwatch = Stopwatch.StartNew();
    }

    public override void OnActionExecuted(ActionExecutedContext context)
    {
        _stopwatch.Stop();
        var elapsedMilliseconds = _stopwatch.ElapsedMilliseconds;
        context.HttpContext.Response.Headers.Add("X-Execution-Time", elapsedMilliseconds.ToString());
    }
}

[ExecutionTimeActionFilter]
public IActionResult Index()
{
    return View();
}
```

Example 2: Validate the model state before executing the action
```csharp
public class ValidateModelAttribute : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext context)
    {
        if (!context.ModelState.IsValid)
        {
            context.Result = new BadRequestObjectResult(context.ModelState);
        }
    }
}

[ValidateModelAttribute]
[HttpPost]
public IActionResult Create(Product product)
{
    // ...
}
```

**4. Result Filters**

Example 1: Modify the action result
```csharp
public class CustomJsonResultFilter : ResultFilterAttribute
{
    public override void OnResultExecuting(ResultExecutingContext context)
    {
        if (context.Result is JsonResult jsonResult)
        {
            jsonResult.SerializerSettings.Formatting = Formatting.Indented;
        }
    }
}

[CustomJsonResultFilter]
public IActionResult GetProduct(int id)
{
    // ...
}
```

Example 2: Add a custom header to the response
```csharp
public class CustomHeaderResultFilter : ResultFilterAttribute
{
    public override void OnResultExecuting(ResultExecutingContext context)
    {
        context.HttpContext.Response.Headers.Add("X-Custom-Header", "HeaderValue");
    }
}

[CustomHeaderResultFilter]
public IActionResult Index()
{
    return View();
}
```

**5. Exception Filters**

Example 1: Log exceptions and return a custom error response
```csharp
public class CustomExceptionFilter : IExceptionFilter
{
    private readonly ILogger _logger;

    public CustomExceptionFilter(ILogger<CustomExceptionFilter> logger)
    {
        _logger = logger;
    }

    public void OnException(ExceptionContext context)
    {
        _logger.LogError(context.Exception, "An error occurred");

        var errorResponse = new { error = "An error occurred while processingyour request." };
        context.Result = new JsonResult(errorResponse)
        {
            StatusCode = StatusCodes.Status500InternalServerError
        };
        context.ExceptionHandled = true;
    }
}

[ServiceFilter(typeof(CustomExceptionFilter))]
public class HomeController : Controller
{
    // ...
}

// Register the filter in Startup.cs
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<CustomExceptionFilter>();
    services.AddControllersWithViews();
}
```

Example 2: Handle specific exception types and return customized error responses
```csharp
public class CustomExceptionHandler : IExceptionFilter
{
    public void OnException(ExceptionContext context)
    {
        if (context.Exception is ArgumentNullException)
        {
            context.Result = new JsonResult(new { error = "A required argument was not provided." })
            {
                StatusCode = StatusCodes.Status400BadRequest
            };
        }
        else if (context.Exception is InvalidOperationException)
        {
            context.Result = new JsonResult(new { error = "The requested operation is not valid." })
            {
                StatusCode = StatusCodes.Status400BadRequest
            };
        }
        else
        {
            context.Result = new JsonResult(new { error = "An unexpected error occurred." })
            {
                StatusCode = StatusCodes.Status500InternalServerError
            };
        }
        context.ExceptionHandled = true;
    }
}

[ServiceFilter(typeof(CustomExceptionHandler))]
public class HomeController : Controller
{
    // ...
}

// Register the filter in Startup.cs
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<CustomExceptionHandler>();
    services.AddControllersWithViews();
}
```

These examples demonstrate common use cases for each type of filter in ASP.NET Core, providing you with a good starting point for implementing filters in your own web applications or web APIs.

## Filters vs Middlewares

Filters and middlewares are both mechanisms used in ASP.NET Core to handle and modify HTTP requests and responses. However, they have different purposes, scopes, and are used at different stages of the request pipeline. Here's a simple comparison to help you understand the differences between filters and middlewares:

**Filters**

1. Purpose: Filters are specifically designed for ASP.NET Core MVC and Razor Pages applications. They allow you to perform pre-processing and post-processing of action methods, result execution, exception handling, and other parts of the request pipeline that are specific to MVC and Razor Pages.

2. Scope: Filters are scoped to controllers and action methods, meaning they can be applied to individual controllers or actions to modify their behavior.

3. Execution Order: Filters are executed in a specific order based on their type: Authorization filters, Resource filters, Action filters, Result filters, and Exception filters.

4. Context: Filters have access to the context of the action or the result being executed, providing more detailed information about the current request, such as action arguments, ModelState, ViewData, TempData, and more.

5. Use Cases: Filters are commonly used for tasks like authorization, caching, logging, validation, exception handling, and modifying action results.

**Middlewares**

1. Purpose: Middlewares are more general-purpose and can be used in any ASP.NET Core application, including MVC, Razor Pages, Web API, and others. They allow you to handle and modify incoming HTTP requests and outgoing HTTP responses, acting as a chain of components in the request pipeline.

2. Scope: Middlewares are application-wide, meaning they are applied to every request coming into the application and every response going out.

3. Execution Order: Middlewares are executed in the order they are added to the pipeline, with the request flowing through each middleware component in sequence, and the response flowing back through the same sequence in reverse.

4. Context: Middlewares have access to the HttpContext, which includes the request and response objects, but they do not have direct access to action-specific context or detailed information about MVC or Razor Pages components.

5. Use Cases: Middlewares are commonly used for tasks like request/response logging, routing, authentication, CORS, compression, exception handling, and more.

In summary, filters are primarily used for tasks specific to MVC and Razor Pages applications, while middlewares are more general-purpose and can be used across different types of ASP.NET Core applications. Filters operate within the context of controllers and actions, while middlewares work at the application level, processing all incoming requests and outgoing responses.

---

In summary, filters and attributes in ASP.NET Core provide a powerful way to introduce cross-cutting concerns and execute custom code at different stages of the request pipeline. They can be applied to controllers, actions, or globally across your application, allowing you to centralize specific behaviors and maintain a clean separation of concerns.

By using built-in attributes or creating your own custom filters, you can implement functionality such as authentication, authorization, caching, input/output manipulation, and exception handling in a modular and reusable way. This helps to improve the maintainability and readability of your code and allows you to easily modify or extend your application's behavior without making extensive changes to your controllers or actions. 