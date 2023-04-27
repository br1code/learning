# Globalization and Localization

Globalization and Localization are essential aspects of building web applications and APIs that cater to a global audience. Globalization is the process of designing applications that can adapt to different cultures, while Localization involves providing translations and culture-specific formatting for the content. ASP.NET Core provides built-in support for both Globalization and Localization.

**1. Configure Supported Cultures:**
First, you need to configure the supported cultures for your application. Add the following code in the `ConfigureServices` method of the `Startup.cs` file:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<RequestLocalizationOptions>(options =>
    {
        var supportedCultures = new List<CultureInfo>
        {
            new CultureInfo("en-US"),
            new CultureInfo("fr-FR"),
            new CultureInfo("es-ES")
        };

        options.DefaultRequestCulture = new RequestCulture("en-US");
        options.SupportedCultures = supportedCultures;
        options.SupportedUICultures = supportedCultures;
    });

    // Other services
    services.AddControllers();
}
```

In this example, we've configured the application to support English (United States), French (France), and Spanish (Spain) cultures.

**2. Add Request Localization Middleware:**
Next, add the Request Localization middleware to the `Configure` method in the `Startup.cs` file:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    // Other middlewares
    var localizationOptions = app.ApplicationServices.GetService<IOptions<RequestLocalizationOptions>>().Value;
    app.UseRequestLocalization(localizationOptions);

    app.UseRouting();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

The Request Localization middleware sets the current thread's culture based on the incoming request's culture, using the `Accept-Language` HTTP header or a culture-specific route value or query string.

**3. Create Resource Files:**
Resource files are used to store translated content for different languages. By convention, resource files are named using the pattern `ResourceName.[Culture].resx`. Create a `Resources` folder in your project and add a resource file named `SharedResources.resx` for the default culture (English) and additional resource files for the supported cultures, such as `SharedResources.fr-FR.resx` and `SharedResources.es-ES.resx`. Add key-value pairs to the resource files for translated content.

**4. Configure Resource-Based Localization:**
To use resource-based localization, you need to configure it in the `ConfigureServices` method of the `Startup.cs` file:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Localization configuration
    services.AddLocalization(options => options.ResourcesPath = "Resources");

    services.AddControllers()
            .AddViewLocalization(LanguageViewLocationExpanderFormat.Suffix)
            .AddDataAnnotationsLocalization();

    // Other services
}
```

Here, we've configured the `ResourcesPath` to the `Resources` folder, added view localization with a language-specific suffix, and added Data Annotations localization for model validation.

**5. Inject IStringLocalizer and Use Translated Strings:**
Now, you can inject the `IStringLocalizer<T>` interface in your controllers, views, or Razor Pages to access the translated strings. Here's an example of using `IStringLocalizer` in a controller:

```csharp
public class HomeController : Controller
{
    private readonly IStringLocalizer<SharedResources> _localizer;

    public HomeController(IStringLocalizer<SharedResources> localizer)
    {
        _localizer = localizer;
    }

    public IActionResult Index()
    {
        ViewBag.Message = _localizer["HelloWorld"];
        return View();
    }
}
```

**6. Use Localization in Views:**
You can also inject `IViewLocalizer` in your Razor views to localize content. Here's an example of using `IViewLocalizer` in a Razor view:

```html
@using Microsoft.AspNetCore.Mvc.Localization
@inject IViewLocalizer Localizer

<h1>@Localizer["HelloWorld"]</h1>
```

This code snippet injects the `IViewLocalizer` and uses it to display a localized "HelloWorld" message.

**7. Localize Data Annotations:**
ASP.NET Core supports localizing data annotations for model validation. To do this, add the `[Display(Name = "ResourceKey")]` attribute to your model properties and use the `IStringLocalizer<T>` in your views to display validation error messages. Here's an example:

```csharp
public class RegisterViewModel
{
    [Required(ErrorMessageResourceType = typeof(SharedResources), ErrorMessageResourceName = "EmailRequired")]
    [EmailAddress(ErrorMessageResourceType = typeof(SharedResources), ErrorMessageResourceName = "EmailInvalid")]
    [Display(Name = "Email")]
    public string Email { get; set; }

    // Other properties
}
```

In your Razor view, use the `IStringLocalizer<T>` to display the validation error messages:

```html
@using Microsoft.AspNetCore.Mvc.Localization
@inject IViewLocalizer Localizer

<div asp-validation-summary="All" class="text-danger">@Localizer["ValidationErrors"]</div>
```

**8. Set Culture Programmatically:**
In some cases, you might want to allow users to select their preferred language or culture. You can store the user's preference in a cookie or database and set the culture programmatically. To do this, add a new action method to your controller that sets the culture and redirects the user to a specific page:

```csharp
public IActionResult SetLanguage(string culture, string returnUrl)
{
    Response.Cookies.Append(
        CookieRequestCultureProvider.DefaultCookieName,
        CookieRequestCultureProvider.MakeCookieValue(new RequestCulture(culture)),
        new CookieOptions { Expires = DateTimeOffset.UtcNow.AddYears(1) }
    );

    return LocalRedirect(returnUrl);
}
```

In this example, the `SetLanguage` action sets the user's preferred culture in a cookie and redirects them back to the provided `returnUrl`.

In conclusion, ASP.NET Core provides powerful features for implementing Globalization and Localization in your web applications and APIs. By configuring supported cultures, setting up resource files, and using the `IStringLocalizer<T>` and `IViewLocalizer` services, you can create applications that cater to a global audience, providing translated content and culture-specific formatting.