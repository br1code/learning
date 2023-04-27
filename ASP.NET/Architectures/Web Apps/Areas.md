# Areas

Areas in ASP.NET Core provide a way to organize a web application into logical, functional, and maintainable units. They are particularly useful for large applications with multiple distinct features or modules, where separating these features into their own namespaces and folders can make the codebase more manageable. Areas allow you to group controllers, views, and other components together under a common namespace, keeping each feature's code in a separate folder.

To create an area in an ASP.NET Core application, follow these steps:

1. **Create an "Areas" folder in your project**: The first step is to create a folder named "Areas" in the root directory of your project. This folder will contain all the areas in your application.

2. **Create a folder for the area**: Inside the "Areas" folder, create a new folder for your area. The folder name should match the desired area name, e.g., "Admin", "Users", or "Products".

3. **Create a folder structure within the area folder**: Inside the area folder, create the following folder structure:

```
AreaName
│
├─ Controllers
├─ Views
│   └─ Shared
└─ Models
```

These folders will contain the controllers, views, and models specific to the area.

4. **Create a "_ViewStart.cshtml" file in the "Views" folder**: In the "Views" folder of the area, create a "_ViewStart.cshtml" file with the following content:

```html
@{
    Layout = "/Views/Shared/_Layout.cshtml";
}
```

This ensures that the views within this area use the shared layout from the main application.

5. **Create a controller**: In the "Controllers" folder, create a new controller specific to the area. Make sure to set the namespace for the controller to include the area name:

```csharp
using Microsoft.AspNetCore.Mvc;

namespace YourApp.Areas.AreaName.Controllers
{
    [Area("AreaName")]
    public class HomeController : Controller
    {
        public IActionResult Index()
        {
            return View();
        }
    }
}
```

Notice the `[Area("AreaName")]` attribute, which specifies the area name for the controller.

6. **Create a view**: In the "Views" folder, create a new view specific to the area. For example, create a "Home" folder inside the "Views" folder and add an "Index.cshtml" file:

```html
@{
    ViewData["Title"] = "AreaName Home Page";
}

<h1>@ViewData["Title"]</h1>
```

7. **Update the routing configuration**: In the "Startup.cs" file, update the routing configuration to include areas:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "areas",
        pattern: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
});
```

This configuration adds a route for areas, ensuring that the URLs are in the format `/AreaName/Controller/Action`.

With these steps, you have created a new area in your ASP.NET Core application. You can now access the area's "Home" controller and "Index" action by navigating to the URL `/AreaName/Home/Index`.

Areas help you to better organize large projects, separating different functional areas into their own folders and namespaces. This makes it easier to manage and maintain the application, as each area can be developed, tested, and deployed independently.

**Note:** While areas are typically associated with web applications that have views, you can also use areas in Web API applications to organize your controllers and provide a logical separation between different parts of your API.