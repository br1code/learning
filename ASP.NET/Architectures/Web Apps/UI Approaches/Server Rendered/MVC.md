# MVC

## Introduction

ASP.NET Core MVC is a web application development framework built on top of ASP.NET Core, designed to enable developers to create dynamic, scalable, and maintainable web applications. It follows the Model-View-Controller (MVC) architectural pattern, which separates an application's logic into three interconnected components, making it easier to manage, test, and evolve the codebase.

## Simple explanation of the Model-View-Controller architecture

The Model-View-Controller (MVC) architecture is a design pattern that separates an application's logic into three main components:

1. Model: Represents the data and business logic of the application. It is responsible for retrieving and storing data, as well as performing any necessary data processing.

2. View: Represents the user interface and presentation of the data. It is responsible for displaying the data provided by the Model in a user-friendly manner.

3. Controller: Acts as an intermediary between the Model and View, receiving user input from the View and processing it by interacting with the Model. The Controller then updates the View with the results.

This separation of concerns allows for easier maintenance, testing, and evolution of the application, as each component can be developed and modified independently of the others.

## Benefits of using ASP.NET Core MVC

Some of the benefits of using ASP.NET Core MVC include:

1. Separation of Concerns: The MVC architecture promotes a clear separation of concerns, making it easier to manage, test, and evolve the codebase.

2. Testability: The modular nature of MVC components allows for better unit testing and integration testing, leading to more robust and reliable applications.

3. Reusability: The separation of components makes it easier to reuse code across different parts of the application or even across multiple applications.

4. Scalability: ASP.NET Core MVC is built on top of the high-performance ASP.NET Core framework, which provides excellent performance and scalability out of the box.

5. Extensibility: The framework offers many extension points and customization options, allowing developers to tailor the behavior of the framework to suit their specific needs.

6. Cross-platform support: ASP.NET Core MVC can run on multiple platforms, such as Windows, macOS, and Linux, making it a versatile choice for web application development.

## Understanding the project structure

An ASP.NET Core MVC project is organized into a set of folders and files that adhere to a specific structure. Some of the key folders and their purposes include:

- Controllers: Contains the controller classes responsible for handling user input, interacting with models, and updating views.

- Models: Contains the classes representing the data and business logic of the application.

- Views: Contains the Razor templates used to render the user interface. Views are organized into folders that correspond to the controllers, with each folder containing views for the respective controller's actions.

- wwwroot: Contains the static files, such as CSS, JavaScript, and images, used by the application.

- appsettings.json: Contains the configuration settings for the application, such as database connection strings and feature flags.

- Program.cs: Contains the application startup configuration, including the registration of services, middleware, and routing.

These folders and files form the foundation of an ASP.NET Core MVC project, providing a clear organization and structure to facilitate efficient development and maintenance.

## Controllers

Controllers in ASP.NET Core MVC are responsible for handling incoming HTTP requests, processing user input, interacting with models, and returning appropriate views or data. Controllers are typically stored in the "Controllers" folder and should inherit from the `Controller` base class provided by the framework.

Here's an example of a simple controller with two actions:

```csharp
using Microsoft.AspNetCore.Mvc;
using MyApp.Models;

namespace MyApp.Controllers
{
    public class HomeController : Controller
    {
        public IActionResult Index()
        {
            return View();
        }

        public IActionResult Details(int id)
        {
            var model = new MyModel
            {
                Id = id,
                Name = "Example"
            };

            return View(model);
        }
    }
}
```

In this example, we have a `HomeController` with two actions: `Index` and `Details`. The `Index` action returns the default view without any model data, while the `Details` action creates a new instance of `MyModel` and passes it to the view.

## Models

Models in ASP.NET Core MVC represent the data and business logic of the application. They are typically stored in the "Models" folder and can be simple data classes or more complex classes that interact with databases or external services.

Here's an example of a simple model:

```csharp
namespace MyApp.Models
{
    public class MyModel
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }
}
```

In this example, we have a `MyModel` class with two properties: `Id` and `Name`. This model can be used by the controller to pass data between the view and the application's business logic or data storage.

## Views

Views in ASP.NET Core MVC are responsible for rendering the user interface and displaying the data provided by the controller. Views are usually written using the Razor templating language, which allows for a mix of HTML and C# code. Views are stored in the "Views" folder and organized into subfolders corresponding to the controllers.

Here's an example of a view for the `Details` action in the `HomeController`:

`Views/Home/Details.cshtml`

```html
@model MyApp.Models.MyModel

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Details</title>
</head>
<body>
    <h1>Details for @Model.Name</h1>
    <p>ID: @Model.Id</p>
    <p>Name: @Model.Name</p>
</body>
</html>
```

In this example, we have a Razor view that displays the details of the `MyModel` instance passed by the `Details` action in the `HomeController`. The `@model` directive at the top of the file specifies the type of the model being used by the view, and the `@Model` keyword is used to access the model data within the HTML markup.

Together, controllers, models, and views form the core components of the ASP.NET Core MVC architecture, allowing for efficient development and maintenance of web applications.

## Program.cs

This is how a `Program.cs` file for a MVC app would look like:

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllersWithViews();

var app = builder.Build();

app.UseStaticFiles();
app.UseRouting();

app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

app.Run();
```