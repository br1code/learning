# Razor Pages

**TODO: review and update with modern versions**

Razor Pages is a feature of ASP.NET Core that provides a simplified programming model for creating web applications. It's built on top of the Razor engine and is an alternative to the traditional ASP.NET Core MVC approach. Razor Pages is designed to be more straightforward and easy to use, making it an excellent choice for developers who are building simple web applications or who are new to ASP.NET Core.

## Razor Pages Structure

Razor Pages organizes web application functionality into individual pages, each consisting of two files:

1. A Razor View file (`.cshtml`): This file contains the HTML markup and Razor syntax needed to render the page.
2. A Page Model file (`.cshtml.cs`): This is a C# class file that provides the server-side logic for the page.

These files are typically organized within the `Pages` folder of an ASP.NET Core project. The Page Model file is named with the same name as the Razor View file, but with the `.cshtml.cs` extension.

This is how a `Program.cs` file for a Razor Pages app would look like:

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorPages();

var app = builder.Build();

app.UseStaticFiles();
app.UseRouting();

app.MapRazorPages();

app.Run();
```

## Page Model

The Page Model is a C# class that provides the server-side logic for a Razor Page. It should inherit from the `PageModel` base class and can define properties, methods, and event handlers to manage the page's state and handle user interactions.

Here's a simple example of a Page Model:

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

public class IndexModel : PageModel
{
    public string Message { get; set; }

    public void OnGet()
    {
        Message = "Hello, World!";
    }
}
```

In this example, the `IndexModel` class inherits from `PageModel` and defines a `Message` property. The `OnGet()` method is an event handler that is automatically called when the page is requested using an HTTP GET request. In this case, the method sets the `Message` property to "Hello, World!".

## Handling User Input

Razor Pages can handle user input by defining properties with the `[BindProperty]` attribute in the Page Model. This attribute tells the framework to automatically bind the property's value from the incoming HTTP request.

Here's an example of a Page Model that binds a user's input:

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

public class IndexModel : PageModel
{
    [BindProperty]
    public string UserName { get; set; }

    public string Message { get; set; }

    public void OnGet()
    {
        Message = $"Hello, {UserName}!";
    }

    public IActionResult OnPost()
    {
        Message = $"Hello, {UserName}!";
        return Page();
    }
}
```

In this example, the `UserName` property is decorated with the `[BindProperty]` attribute, allowing it to be automatically populated from the HTTP request. The `OnPost()` method is an event handler that is called when the page is submitted using an HTTP POST request. In this case, the method sets the `Message` property based on the `UserName` property and returns the current page.

**Routing**

Razor Pages uses a simple, convention-based routing system. By default, the route for a Razor Page is determined by its location within the `Pages` folder. For example, a Razor Page located at `Pages/Index.cshtml` would have a route of `/Index`. Similarly, a Razor Page located at `Pages/Products/List.cshtml` would have a route of `/Products/List`.

You can also customize the route for a Razor Page by adding the `@page` directive with a custom route template at the beginning of the Razor View file:

```html
@page "/my-custom-route"
```

## Layouts, Sections, and Partial Views

Razor Pages supports the same layout, sections, and partial view features as ASP.NET Core MVC. This means you can create a consistent layout across your application, define optional or required sections within your layout, and reuse portions of your views using partial views.

1. Layouts: To use a layout in a Razor Page, set the `Layout` property at the top of the Razor View file:

`Pages/Index.cshtml`

```html
@page
@model IndexModel
@{
    Layout = "_Layout";
}

<h2>@Model.Message</h2>
<p>This content will be inserted into the layout's @RenderBody() location.</p>
```

2. Sections: Sections can be defined within layouts using the `@RenderSection()` method, just like in ASP.NET Core MVC. To provide content for a section in a Razor Page, use the `@section` directive, followed by the section name and the content within curly braces `{...}`.

3. Partial Views: Partial views can be used in Razor Pages just like in ASP.NET Core MVC. To render a partial view within a Razor Page, use the `@Html.Partial()` or `@await Html.PartialAsync()` methods. Similarly, you can also use View Components in Razor Pages by calling `@Component.Invoke()` or `@await Component.InvokeAsync()`.

## Handling Page Lifecycle Events

In addition to handling user input, Razor Pages can also respond to different events in the page's lifecycle by implementing event handlers in the Page Model. The most common event handlers are `OnGet()` and `OnPost()`, which are executed when the page is requested using an HTTP GET or POST request, respectively.

However, Razor Pages also supports other event handlers for various HTTP verbs:

- `OnGetAsync()`: Called for an asynchronous GET request.
- `OnPostAsync()`: Called for an asynchronous POST request.
- `OnPut()`, `OnPutAsync()`: Called for PUT requests.
- `OnDelete()`, `OnDeleteAsync()`: Called for DELETE requests.

These event handlers can be used to execute specific logic based on the HTTP verb used in the request.

## Razor Pages vs. ASP.NET Core MVC

While Razor Pages and ASP.NET Core MVC share many similarities, they cater to different scenarios and developer preferences. Razor Pages provides a more straightforward approach to building web applications, focusing on a single page and its associated logic. This can be beneficial for simple applications or when developers prefer organizing code around pages rather than the MVC pattern.

On the other hand, ASP.NET Core MVC provides a more structured approach, separating application logic into Models, Views, and Controllers. This separation can make it easier to manage complex applications with many components and can be preferred by developers who are comfortable with the MVC pattern.

In summary, Razor Pages is a feature of ASP.NET Core that simplifies web application development by focusing on individual pages and their associated logic. It offers a more straightforward and easy-to-understand programming model compared to the traditional ASP.NET Core MVC approach, making it an excellent choice for building simple web applications or for developers who are new to ASP.NET Core.

## Validation in Razor Pages

Razor Pages supports validation using data annotations on the properties of the Page Model. These annotations enable you to define validation rules, display error messages, and ensure that user input meets your application's requirements.

To enable validation, add the appropriate data annotation attributes to the properties in your Page Model, and use the `@Html.ValidationMessageFor()` helper method in your Razor View to display validation error messages.

Here's an example of adding validation to a Razor Page:

`Pages/Contact.cshtml.cs`

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

public class ContactModel : PageModel
{
    [Required(ErrorMessage = "Name is required.")]
    [StringLength(50, ErrorMessage = "Name must be less than 50 characters.")]
    [BindProperty]
    public string Name { get; set; }

    [Required(ErrorMessage = "Email is required.")]
    [EmailAddress(ErrorMessage = "Invalid email address.")]
    [BindProperty]
    public string Email { get; set; }

    [Required(ErrorMessage = "Message is required.")]
    [BindProperty]
    public string Message { get; set; }

    public IActionResult OnPost()
    {
        if (!ModelState.IsValid)
        {
            return Page();
        }

        // Process the form submission...
        return RedirectToPage("Success");
    }
}
```

`Pages/Contact.cshtml`

```html
@page
@model ContactModel
@{
    Layout = "_Layout";
}

<h2>Contact Us</h2>

<form method="post">
    <div>
        <label for="Name">Name:</label>
        <input id="Name" name="Name" type="text" value="@Model.Name" />
        <span>@Html.ValidationMessageFor(m => m.Name)</span>
    </div>
    <div>
        <label for="Email">Email:</label>
        <input id="Email" name="Email" type="email" value="@Model.Email" />
        <span>@Html.ValidationMessageFor(m => m.Email)</span>
    </div>
    <div>
        <label for="Message">Message:</label>
        <textarea id="Message" name="Message">@Model.Message</textarea>
        <span>@Html.ValidationMessageFor(m => m.Message)</span>
    </div>
    <button type="submit">Send</button>
</form>
```

In this example, the `Name`, `Email`, and `Message` properties have various data annotation attributes, such as `[Required]`, `[EmailAddress]`, and `[StringLength]`, to enforce validation rules. The Razor View uses the `@Html.ValidationMessageFor()` helper method to display validation error messages next to the corresponding input fields.

## AJAX and Razor Pages

Razor Pages can be used in combination with AJAX to create responsive, dynamic web applications. You can use JavaScript to make AJAX requests to your Razor Pages, allowing you to update the page content without requiring a full page refresh.

To implement AJAX in Razor Pages, follow these steps:

1. Create a JavaScript function to make an AJAX request to the desired Razor Page, passing any required data as parameters.
2. In the Page Model, add a new event handler to process the AJAX request and return the desired data as a JSON object.
3. Update the JavaScript function to handle the JSON response and update the page content accordingly.

Here's an example of using AJAX to update a list of items on a Razor Page:

`Pages/Items.cshtml`

```html
@page
@model ItemsModel
@{
    Layout = "_Layout";
}

<h2>Items</h2>

<ul id="items-list">
    @foreach (var item in Model.Items)
    {
        <li>@item.Name</li>
    }
</ul>

<button id="load-items">Load More Items</button>

@section Scripts {
    <script>
        document.getElementById("load-items").addEventListener("click", function () {
            loadMoreItems();
        });

        function loadMoreItems() {
            fetch("/Items?handler=LoadMoreItems")
                .then(response => response.json())
                .then(data => {
                    let itemsList = document.getElementById("items-list");
                    data.forEach(item => {
                        let listItem = document.createElement("li");
                        listItem.textContent = item.name;
                        itemsList.appendChild(listItem);
                    });
                });
        }
    </script>
}
```

`Pages/Items.cshtml.cs`

```csharp
using System.Collections.Generic;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

public class ItemsModel : PageModel
{
    public List<Item> Items { get; set; }

    public void OnGet()
    {
        // Load initial items...
        Items = GetItems(0, 10);
    }

    public JsonResult OnGetLoadMoreItems()
    {
        // Load more items...
        var moreItems = GetItems(Items.Count, 10);
        return new JsonResult(moreItems);
    }

    private List<Item> GetItems(int start, int count)
    {
        // Replace with logic to load items from a data source...
        var items = new List<Item>();
        for (int i = start; i < start + count; i++)
        {
            items.Add(new Item { Name = $"Item {i}" });
        }
        return items;
    }

    public class Item
    {
        public string Name { get; set; }
    }
}
```

In this example, the `Items.cshtml` Razor View includes a JavaScript function `loadMoreItems()` that makes an AJAX request to the `/Items?handler=LoadMoreItems` endpoint when the "Load More Items" button is clicked. The `ItemsModel` Page Model includes an `OnGetLoadMoreItems()` event handler that returns a JSON array of additional items. The JavaScript function then processes the JSON response and updates the items list on the page.

## Razor Pages and Dependency Injection

Razor Pages supports Dependency Injection (DI) just like ASP.NET Core MVC. You can use constructor injection to provide your Page Models with the services and dependencies they need.

To use Dependency Injection in Razor Pages, simply add the required service types as parameters in the constructor of your Page Model class, and the ASP.NET Core framework will automatically provide the necessary instances.

Here's an example of using Dependency Injection in a Razor Page to access a custom `IItemService`:

`Pages/ItemList.cshtml.cs`

```csharp
using System.Collections.Generic;
using Microsoft.AspNetCore.Mvc.RazorPages;
using MyApp.Services;

public class ItemListModel : PageModel
{
    private readonly IItemService _itemService;

    public List<Item> Items { get; set; }

    public ItemListModel(IItemService itemService)
    {
        _itemService = itemService;
    }

    public void OnGet()
    {
        Items = _itemService.GetItems();
    }
}
```

In this example, the `ItemListModel` Page Model has a constructor that accepts an `IItemService` instance, which is automatically provided by the ASP.NET Core Dependency Injection system. The `OnGet()` method then uses the `_itemService` to retrieve the list of items.

## Conclusion

In summary, Razor Pages is a powerful and flexible feature of ASP.NET Core that simplifies web application development. It supports validation, AJAX, Dependency Injection, and all the essential features needed to build modern, dynamic web applications.
   