# Blazor

**TODO: review and update with modern versions**

ASP.NET Core Blazor is a framework for building interactive web applications using C# and Razor syntax. With Blazor, you can write both client-side and server-side logic in C#, reducing the need for JavaScript. Blazor uses WebAssembly to run C# code directly in the browser or can be run on the server and utilize SignalR to communicate with the client.

## Blazor Components

Blazor applications are built using components. A component is a reusable piece of UI that includes both HTML markup and C# code. Components can be nested, reused, and shared across projects. A component's C# code is defined within an `@code` block or a separate C# file with the same name as the component.

Here's a simple example of a Blazor component:

`Counter.razor`

```html
<h3>Counter</h3>

<p>Current count: @currentCount</p>

<button @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

In this example, a `Counter` component displays the current count and provides a button to increment the count. The `@onclick` attribute is used to bind the `IncrementCount` method to the button click event.

## Component Parameters

Components can accept parameters, allowing you to pass data and customize the component's behavior. To define a parameter, create a public property in the component's `@code` block with the `[Parameter]` attribute.

Here's an example of a component with a parameter:

`Greeting.razor`

```html
<p>Hello, @Name!</p>

@code {
    [Parameter]
    public string Name { get; set; }
}
```

You can use the `Greeting` component in another component and pass the `Name` parameter like this:

`ParentComponent.razor`

```html
<Greeting Name="John" />
```

## Dependency Injection

Blazor supports dependency injection, allowing you to provide services to your components. To inject a service into a Blazor component, use the `@inject` directive followed by the service type and a property name.

Example of injecting a service into a Blazor component:

`WeatherForecastService.cs`

```csharp
public class WeatherForecastService
{
    public Task<WeatherForecast[]> GetForecastAsync()
    {
        // ...
    }
}
```

`WeatherComponent.razor`

```html
@page "/weather"
@inject WeatherForecastService ForecastService

<h1>Weather Forecast</h1>

@if (forecasts == null)
{
    <p><em>Loading...</em></p>
}
else
{
    // Display weather forecasts
}

@code {
    private WeatherForecast[] forecasts;

    protected override async Task OnInitializedAsync()
    {
        forecasts = await ForecastService.GetForecastAsync();
    }
}
```

In this example, the `WeatherForecastService` is injected into the `WeatherComponent`, and the `OnInitializedAsync` method is used to retrieve the weather forecast data.

## Routing

Blazor includes built-in support for client-side routing. To define a route for a component, use the `@page` directive followed by the route template.

Here's an example of a component with a route:

`UserProfile.razor`

```razor
@page "/user/{Id:int}"

<p>User profile for user with ID: @Id</p>

@code {
    [Parameter]
    public int Id { get; set; }
}
```

In this example, the `UserProfile` component has a route that includes a route parameter `Id`. The `Id` parameter is also defined as a component parameter with the `[Parameter]` attribute, allowing it to receive the value from the route.

## Event Handling

Blazor components can handle user events, such as button clicks or form submissions, using C# event handlers. Event handlers can be bound to UI events using the `@` symbol followed by the event name, such as `@onclick` for a button click event.

Here's an example of handling a button click event in a Blazor component:

`ButtonClickExample.razor`

```razor
<button @onclick="OnButtonClick">Click me!</button>

<p>@message</p>

@code {
    private string message;

    private void OnButtonClick()
    {
        message = "Button clicked!";
    }
}
```

In this example, the `OnButtonClick` method is bound to the button's click event. When the button is clicked, the `message` property is updated, and the new message is displayed on the page.

## Two-way Data Binding

Blazor components support two-way data binding, which automatically updates the component's state and the UI when the user interacts with form elements. To enable two-way data binding, use the `@bind` directive followed by the property you want to bind to the form element.

Here's an example of two-way data binding in a Blazor component:

`DataBindingExample.razor`

```razor
<p>Name: @name</p>
<input @bind="name" type="text" />

@code {
    private string name;
}
```

In this example, the `name` property is bound to the input element using the `@bind` directive. When the user updates the input value, the `name` property is automatically updated and displayed on the page.

## JavaScript Interop

Blazor allows you to call JavaScript functions from C# code and vice versa. This is useful for integrating with existing JavaScript libraries or utilizing browser APIs not yet available in WebAssembly.

Here's an example of calling a JavaScript function from a Blazor component:

`wwwroot/js/alert.js`

```javascript
function showAlert(message) {
    alert(message);
}
```

`AlertButton.razor`

```razor
@inject IJSRuntime JSRuntime

<button @onclick="ShowAlert">Show Alert</button>

@code {
    private async Task ShowAlert()
    {
        await JSRuntime.InvokeVoidAsync("showAlert", "Hello from Blazor!");
    }
}
```

In this example, the `showAlert` JavaScript function is defined in `alert.js`. The `AlertButton` component uses the `IJSRuntime` service to call the `showAlert` function when the button is clicked.

## Project Structure

A Blazor app typically has the following structure:

```
BlazorApp
│
├─ wwwroot                   // Contains static files like CSS, JavaScript, and images
│   ├── css
│   ├── js
│   └── ...
│
├─ Shared                    // Contains shared components and layouts
│   ├── MainLayout.razor
│   ├── NavMenu.razor
│   └── ...
│
├─ Pages                     // Contains components that represent pages with routing
│   ├── Index.razor
│   ├── Counter.razor
│   ├── FetchData.razor
│   └── ...
│
├─ Services                  // Contains services like data access and business logic
│   ├── WeatherForecastService.cs
│   └── ...
│
├─ _Imports.razor            // Contains common using directives and component imports
├─ App.razor                 // Contains the root component and router configuration
└─ Program.cs                // Contains the application entry point and configuration
```

1. `wwwroot`: This folder contains the static files for the application, such as CSS, JavaScript, and images. These files are served directly by the webserver and can be referenced from the Blazor components.

2. `Shared`: This folder typically contains shared components and layouts used throughout the application. For example, `MainLayout.razor` is the default layout for the application, and `NavMenu.razor` is a reusable navigation menu component.

3. `Pages`: This folder contains components that represent pages in the application. These components usually have an associated route defined using the `@page` directive. Examples include `Index.razor`, which represents the home page, `Counter.razor` for a simple counter example, and `FetchData.razor` for displaying fetched data from a service.

4. `Services`: This folder contains services that handle data access, business logic, and other concerns that are not directly related to the UI. Services can be injected into components using dependency injection.

5. `_Imports.razor`: This file contains common `using` directives and component imports that are shared across the entire application. It helps to avoid repeating the same `using` directives in multiple components.

6. `App.razor`: This file contains the root component and router configuration for the Blazor application. It is responsible for setting up the layout, handling routing, and rendering the appropriate components based on the current route.

7. `Program.cs`: This file contains the application entry point and configuration. It is responsible for setting up the dependency injection container, configuring the application, and starting the Blazor app.

The structure of a Blazor app can be further customized and organized according to the specific requirements and complexity of your project. However, the above structure represents a typical starting point for a Blazor application.

## Conclusion

In summary, ASP.NET Core Blazor is a powerful framework for building interactive web applications using C# and Razor. It provides a rich set of features, including components, routing, event handling, two-way data binding, and JavaScript interop. With Blazor, you can build modern, responsive web applications while leveraging your existing C# skills and reducing the need for JavaScript.