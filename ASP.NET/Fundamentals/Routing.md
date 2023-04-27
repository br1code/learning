# Routing

Routing is a fundamental part of ASP.NET Core, responsible for mapping incoming HTTP requests to the appropriate action methods in your controllers. The routing system in ASP.NET Core is highly configurable and extensible, allowing you to define custom routing rules and conventions.

**1. Attribute Routing:**

Attribute routing is a technique where you apply attributes directly to your controller actions to define the routes. This is the recommended approach in ASP.NET Core, as it provides more control over routing and makes it easier to see the routes directly in your controllers.

Here's an example of attribute routing:

```csharp
[Route("api/[controller]")]
public class UsersController : ControllerBase
{
    [HttpGet("{id}")]
    public IActionResult GetUser(int id)
    {
        // Retrieve the user and return it
    }

    [HttpPost]
    public IActionResult CreateUser([FromBody] User user)
    {
        // Create the user and return the result
    }
}
```

In this example, the `api/[controller]` route template is applied to the `UsersController` class, and the `HttpGet` and `HttpPost` attributes define the HTTP methods and route templates for the `GetUser` and `CreateUser` actions, respectively.

**2. Conventional Routing:**

Conventional routing is a technique where you define your routes in a central location, typically in the `Startup.cs` file. This approach is less common in ASP.NET Core, but can still be useful in some cases.

Here's an example of conventional routing:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
}

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllerRoute(
            name: "default",
            pattern: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

In this example, we define a default route pattern with placeholders for the controller, action, and an optional `id` parameter. The routing system will automatically map incoming requests to the appropriate controller actions based on this pattern.

**3. Route Constraints:**

Route constraints allow you to further restrict the route patterns by specifying conditions that must be met for the route to match. Constraints can be applied using the `{parameter:constraint}` syntax in your route templates.

Here's an example of using route constraints:

```csharp
[Route("api/users/{id:int:min(1)}")]
public IActionResult GetUser(int id)
{
    // Retrieve the user and return it
}
```

In this example, the route constraint `int:min(1)` specifies that the `id` parameter must be an integer with a minimum value of 1. If the constraint is not met, the route will not match, and the request will be handled by another action or result in a 404 error.

**4. Route Tokens:**

Route tokens are special placeholders that can be used in your route templates to automatically generate route values based on conventions. The most common route tokens are `[controller]`, `[action]`, and `[area]`, which represent the controller name, action name, and area name, respectively.

Here's an example of using route tokens:

```csharp
[Route("api/[controller]")]
public class UsersController : ControllerBase
{
    // Controller actions
}
```

In this example, the `[controller]` token is replaced with the name of the controller class (minus the "Controller" suffix), resulting in a route template of `api/users`.

**5. Route Order:**

The order in which routes are evaluated is important, as the first route that matches the incoming request will be used. To control the order of route evaluation, you can set the `Order` property on your route attributes:

```csharp
```csharp
[Route("api/users/{id:int:min(1)}", Order = 1)]
public IActionResult GetUserById(int id)
{
    // Retrieve the user by ID and return it
}

[Route("api/users/{username}", Order = 2)]
public IActionResult GetUserByUsername(string username)
{
    // Retrieve the user by username and return it
}
```

In this example, the `GetUserById` route has an order of 1, and the `GetUserByUsername` route has an order of 2. This ensures that the `GetUserById` route is evaluated before the `GetUserByUsername` route. If the `id` parameter is an integer, the first route will match; otherwise, the second route will be used.

**6. Route Areas:**

Areas in ASP.NET Core allow you to organize your application into logical sections, each with its own set of controllers and views. To use areas, you can create a folder named `Areas` in your project, and then create separate folders for each area. Each area should have its own `Controllers`, `Views`, and optionally `Models` folders.

To define a route for an area, you can use the `[Area]` attribute on your controllers:

```csharp
[Area("Admin")]
[Route("[area]/[controller]")]
public class UsersController : Controller
{
    // Controller actions
}
```

In this example, the `UsersController` is part of the "Admin" area, and the route template includes the `[area]` token. The resulting route will be `Admin/Users`.

**7. Custom Route Constraints:**

You can create custom route constraints by implementing the `IRouteConstraint` interface and registering your custom constraint in the `Startup.cs` file.

Here's an example of a custom route constraint:

```csharp
public class EvenConstraint : IRouteConstraint
{
    public bool Match(HttpContext httpContext, IRouter route, string routeKey,
        RouteValueDictionary values, RouteDirection routeDirection)
    {
        if (values.TryGetValue(routeKey, out var routeValue))
        {
            if (int.TryParse(routeValue.ToString(), out int intValue))
            {
                return intValue % 2 == 0;
            }
        }

        return false;
    }
}

public void ConfigureServices(IServiceCollection services)
{
    services.Configure<RouteOptions>(options =>
    {
        options.ConstraintMap.Add("even", typeof(EvenConstraint));
    });

    services.AddControllersWithViews();
}
```

In this example, we define a custom `EvenConstraint` that only matches even integers. We register the constraint in the `RouteOptions` and then use it in our route templates:

```csharp
[HttpGet("{id:even}")]
public IActionResult GetEvenUser(int id)
{
    // Retrieve the user with an even ID and return it
}
```

This covers the main concepts and features of routing in modern ASP.NET Core. By understanding these concepts and using them effectively, you can create flexible and powerful routing configurations for your web applications and APIs.