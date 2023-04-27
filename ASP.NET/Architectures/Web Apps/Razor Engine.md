# Razor Engine

The Razor engine is a powerful and flexible templating language used in ASP.NET Core applications for rendering dynamic HTML content. It combines C# code with HTML markup, allowing developers to create dynamic web pages that can display data and execute logic. The Razor engine is used primarily within the context of ASP.NET Core MVC Views, but it can also be utilized in other areas, such as Razor Pages and Blazor.

## Syntax

Razor syntax is designed to be easy to read and understand, with minimal overhead. It uses `@` to denote the beginning of a C# code block or expression, allowing you to seamlessly embed C# code within HTML. Here are some of the key aspects of Razor syntax:

1. Inline expressions: To output the value of a C# expression, use the `@` symbol followed by the expression. Razor will evaluate the expression and render its value as part of the HTML markup.

```html
<p>Hello, @User.Identity.Name!</p>
<p>Today is @DateTime.Now.ToShortDateString()</p>
```

2. Code blocks: To define a block of C# code, enclose it between `@{ ... }`. This allows you to declare variables, execute loops, and perform other logic within the view.

```html
@{
    var items = new List<string> { "Apple", "Banana", "Cherry" };
}

<ul>
    @foreach (var item in items)
    {
        <li>@item</li>
    }
</ul>
```

3. Control structures: Razor supports standard C# control structures, such as `if`, `else`, `for`, `foreach`, and `while`. These can be used within code blocks or inline with HTML markup.

```html
@for (int i = 1; i <= 5; i++)
{
    <p>Item @i</p>
}
```

4. HTML encoding: Razor automatically encodes any output generated from C# expressions to protect against cross-site scripting (XSS) attacks. To output raw, unencoded HTML, use the `Html.Raw()` method.

```html
@{
    string htmlContent = "<b>Bold</b>";
}

<p>@Html.Raw(htmlContent)</p>
```

5. Comments: Razor supports both single-line and multi-line comments. Single-line comments start with `@//`, and multi-line comments are enclosed between `@*` and `*@`.

```html
@// This is a single-line comment

@* This is a
    multi-line comment *@
```

## Layouts and Sections

Razor supports layouts and sections to promote code reuse and maintainability. Layouts act as templates that can be used across multiple views, while sections allow you to define content placeholders within layouts that can be filled by individual views.

1. Layouts: To create a layout, define a Razor file with the desired structure and use the `@RenderBody()` method to specify where the content of individual views should be inserted.

   `Views/Shared/_Layout.cshtml`

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>My Application</title>
</head>
<body>
    <header>
        <h1>Welcome to My Application</h1>
    </header>
    <main>
        @RenderBody()
    </main>
    <footer>
        <p>&copy; 2023 My Application</p>
    </footer>
</body>
</html>
```

To use the layout in a view, set the `Layout` property at the top of the view file:

`Views/Home/Index.cshtml`

```html
@{
    Layout = "_Layout";
}

<h2>Welcome to the Home Page</h2>
<p>This content will be inserted into the layout's @RenderBody() location.</p>
```

2. Sections: Sections are defined within layouts using the `@RenderSection()` method and can be given a name to reference them. To make a section optional, pass `required: false` as a second parameter.

`Views/Shared/_Layout.cshtml` (updated)

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>My Application</title>
    @RenderSection("Styles", required: false)
</head>
<body>
    <header>
        <h1>Welcome to My Application</h1>
    </header>
    <main>
        @RenderBody()
    </main>
    <footer>
        <p>&copy; 2023 My Application</p>
    </footer>
    @RenderSection("Scripts", required: false)
</body>
</html>
```

To provide content for a section in a view, use the `@section` directive followed by the section name and the content within curly braces `{...}`:

`Views/Home/Index.cshtml` (updated)

```html
@{
    Layout = "_Layout";
}

@section Styles {
    <style>
        .highlight { font-weight: bold; color: red; }
    </style>
}

<h2 class="highlight">Welcome to the Home Page</h2>
<p>This content will be inserted into the layout's @RenderBody() location.</p>

@section Scripts {
    <script>
        console.log('Custom script for the Home Page');
    </script>
}
```

This allows you to have more control over the placement of specific content, such as styles or scripts, within the layout.

## Razor Helpers

Razor provides several built-in helper methods and properties to facilitate common tasks within views. Some of these include:

1. `Url`: Generates URLs for actions, controllers, or specific routes.
2. `Html`: Provides methods to generate HTML elements, such as forms, inputs, and links.
3. `ViewData` and `ViewBag`: Allow you to store and retrieve data shared between the controller and view.

```html
<a href="@Url.Action("Details", "Home", new { id = 1 })">View Details</a>

@Html.ActionLink("View Details", "Details", "Home", new { id = 1 }, null)

@{
    ViewBag.Title = "Home Page";
}

<h2>@ViewBag.Title</h2>
```

## Partial Views

Partial views are smaller, reusable views that can be embedded within other views. They are useful for modularizing repetitive content, such as headers, footers, or navigation menus. To create a partial view, simply create a Razor file within the "Views" folder or a subfolder, and then use the `@Html.Partial()` or `@await Html.PartialAsync()` methods to render the partial view within another view.

`Views/Shared/_Navigation.cshtml`

```html
<nav>
    <ul>
        <li><a href="/home">Home</a></li>
        <li><a href="/about">About</a></li>
        <li><a href="/contact">Contact</a></li>
    </ul>
</nav>
```

`Views/Shared/_Layout.cshtml` (updated)

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <<title>My Application</title>
    @RenderSection("Styles", required: false)
</head>
<body>
    <header>
        <h1>Welcome to My Application</h1>
        @Html.Partial("_Navigation")
    </header>
    <main>
        @RenderBody()
    </main>
    <footer>
        <p>&copy; 2023 My Application</p>
    </footer>
    @RenderSection("Scripts", required: false)
</body>
</html>
```

In this example, we created a partial view called `_Navigation.cshtml` to represent the navigation menu. We then used the `@Html.Partial()` method to render the partial view within the `_Layout.cshtml` layout file. This allows us to maintain the navigation menu in a separate, reusable file, making it easier to update and maintain.

## View Components

View components are a more powerful alternative to partial views, offering greater flexibility and better encapsulation of logic. They are particularly useful when the content being rendered requires complex logic or data access that should not be handled directly within the view. View components consist of a C# class that inherits from `ViewComponent` and a Razor view for rendering the component.

Here's an example of a simple view component:

`Components/MyViewComponent.cs`

```csharp
using Microsoft.AspNetCore.Mvc;

public class MyViewComponent : ViewComponent
{
    public IViewComponentResult Invoke()
    {
        var data = new List<string> { "Item 1", "Item 2", "Item 3" };
        return View(data);
    }
}
```

`Views/Shared/Components/My/Default.cshtml`

```html
@model List<string>

<ul>
    @foreach (var item in Model)
    {
        <li>@item</li>
    }
</ul>
```

To render the view component within another view, use the `@Component.Invoke()` or `@await Component.InvokeAsync()` methods:

`Views/Home/Index.cshtml`

```html
@{
    Layout = "_Layout";
}

<h2>Welcome to the Home Page</h2>
<p>This content will be inserted into the layout's @RenderBody() location.</p>

@await Component.InvokeAsync("My")
```

In summary, the Razor engine is a powerful templating language that enables developers to create dynamic, data-driven web applications using ASP.NET Core MVC. It provides a rich set of features, including syntax for embedding C# code within HTML markup, layouts and sections for code reuse, Razor helpers for common tasks, partial views and view components for modularizing content, and more.