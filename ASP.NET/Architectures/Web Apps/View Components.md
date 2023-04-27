# View Components

View Components are a feature in ASP.NET Core that allows you to create reusable and modular components for your views. They are similar to Partial Views, but they are more powerful and flexible as they can have their own logic and dependencies.

View Components consist of two parts: a class that inherits from `ViewComponent` and a Razor view that defines the component's HTML markup.

Here's an in-depth explanation of View Components:

1. **Creating a View Component**: To create a View Component, you need to create a new class that inherits from `ViewComponent`. The class should be placed in a folder named `ViewComponents` or have its name end with `ViewComponent`. Here's an example of a simple View Component that displays a list of items:

```csharp
public class ItemListViewComponent : ViewComponent
{
    private readonly IItemRepository _itemRepository;

    public ItemListViewComponent(IItemRepository itemRepository)
    {
        _itemRepository = itemRepository;
    }

    public async Task<IViewComponentResult> InvokeAsync(int maxItems)
    {
        var items = await _itemRepository.GetItemsAsync(maxItems);
        return View(items);
    }
}
```

In this example, we have a View Component named `ItemListViewComponent` that takes an `IItemRepository` dependency and has an `InvokeAsync` method to retrieve and return items.

2. **Creating a View for the View Component**: After creating the View Component class, you need to create a Razor view that defines the HTML markup for the component. The view should be placed in a folder named `Views/Shared/Components/{ComponentName}`. In our example, the folder would be `Views/Shared/Components/ItemList`.

Create a Razor view file named `Default.cshtml` in the folder:

```html
@model IEnumerable<MyApp.Models.Item>

<ul>
    @foreach (var item in Model)
    {
        <li>@item.Name</li>
    }
</ul>
```

3. **Using a View Component**: You can use a View Component in your views or layouts using the `Component.InvokeAsync` method or the `vc:` tag helper. Here's an example of how to use the `ItemListViewComponent` in a Razor view:

Using `Component.InvokeAsync`:

```html
@await Component.InvokeAsync("ItemList", new { maxItems = 5 })
```

Using the `vc:` tag helper:

```html
<vc:item-list max-items="5"></vc:item-list>
```

Both of these methods will render the `ItemList` View Component and pass the `maxItems` parameter to the `InvokeAsync` method.

4. **View Component Caching**: You can cache the output of a View Component to improve performance. To cache a View Component's output, you can use the `[ViewComponentOutputCache]` attribute on the View Component class:

```csharp
[ViewComponentOutputCache(Duration = 600)]
public class ItemListViewComponent : ViewComponent
{
    // ...
}
```

In this example, the output of the `ItemListViewComponent` will be cached for 10 minutes (600 seconds).

View Components in ASP.NET Core provide a powerful and flexible way to create reusable components with their own logic and dependencies for your views.