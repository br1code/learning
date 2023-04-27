# Model Binding and Validation

## Introduction

Model Binding and Model Validation are two important features of ASP.NET Core that help to map incoming request data to controller action parameters and ensure that the data conforms to specified validation rules, respectively.

## Model Binding

Model binding in ASP.NET Core is the process of taking incoming HTTP request data (e.g., from the query string, route data, form data, or request body) and mapping it to the parameters of a controller action method. This simplifies the process of working with data in your actions by automatically converting and assigning the data to the appropriate parameters or properties of your model classes.

When a request is made to an action, the ASP.NET Core framework tries to bind the incoming data to the action's parameters by matching the names of the incoming data keys to the names of the parameters. The model binder also takes care of converting data types, such as converting a string to an integer or a date.

To enable model binding for a specific parameter, you can use various attributes like `[FromQuery]`, `[FromRoute]`, `[FromForm]`, and `[FromBody]`. These attributes help the model binder understand where to look for the data in the request.

Let's take a look at some examples:

**1. `[FromQuery]`:** This attribute is used to bind action parameters from the query string of the request URL.

```csharp
public IActionResult Search([FromQuery] string keyword)
{
    // Perform search using the keyword
}
```

If you call this action with the URL `/Search?keyword=test`, the `keyword` parameter will receive the value "test" from the query string.

**2. `[FromForm]`:** This attribute is used to look for data in the submitted form.

```csharp
[HttpPost]
public IActionResult Create([FromForm] ProductViewModel product)
{
    // Process the product data
}
```

If you send data a POST request to this action using a form, the data with the name `product` will be populated with the deserialized product data.

**3. `[FromRoute]`:** This attribute is used to bind action parameters from the route data.

Assuming you have a route defined like this:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "product",
        pattern: "product/{id}",
        defaults: new { controller = "Product", action = "Details" });
});
```

You can use the `[FromRoute]` attribute in the corresponding action method:

```csharp
public IActionResult Details([FromRoute] int id)
{
    // Get product details using the id
}
```

If you call this action with the URL `/product/42`, the `id` parameter will receive the value 42 from the route data.

**4. `[FromBody]`:** This attribute is used to bind action parameters from the request body, usually in JSON format.

```csharp
[HttpPost]
public IActionResult Create([FromBody] ProductViewModel product)
{
    // Process the product data
}
```

If you send a POST request to this action with a JSON payload containing a product, the `product` parameter will be populated with the deserialized product data.


## Model Validation

Model validation is the process of checking if the data received by a controller action conforms to a set of predefined rules. This ensures that your application processes only valid data and prevents issues caused by invalid or malicious input.

To implement validation rules, you can use data annotations on your model classes. Data annotations are attributes that you can apply to your model properties to define validation constraints, such as required fields, maximum string length, or a range of valid values.

Here's an example of a model with validation rules:

```csharp
public class ProductViewModel
{
    [Required]
    [StringLength(100)]
    public string Name { get; set; }

    [Required]
    [Range(1, 1000)]
    public decimal Price { get; set; }
}
```

In this example, the `Name` property is required and must not exceed 100 characters in length, while the `Price` property is required and must be between 1 and 1000.

To perform validation in your controller action, you can check the `ModelState.IsValid` property. If it's false, the submitted data has violated one or more validation rules, and you can return an appropriate response or view. If it's true, you can proceed with processing the data.

Here's an example:

```csharp
public IActionResult Create(ProductViewModel product)
{
    if (!ModelState.IsValid)
    {
        return View(product);
    }

    // Process the product data
}
```

In this example, the action checks the `ModelState.IsValid` property and returns the view with the submitted data if the validation fails. If the validation is successful, it proceeds to process the product data.

---

**Note:** If you need to validate the model state for multiple actions and want to avoid having duplicated code, you could wrap this logic inside a Filter so the validation happens before executing the action:

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
```

---

Here are some commonly used data annotations for model validation:

**1. `[Required]`:** Indicates that the property must have a value.

```csharp
[Required]
public string Name { get; set; }
```

**2. `[StringLength]`:** Specifies the maximum and/or minimum length for a string property.

```csharp
[StringLength(100, MinimumLength = 10)]
public string Description { get; set; }
```

**3. `[Range]`:** Specifies a range of valid values for a property.

```csharp
[Range(1, 1000)]
public decimal Price { get; set; }
```

**4. `[EmailAddress]`:** Validates that the property value is a valid email address.

```csharp
[EmailAddress]
public string Email { get; set; }
```

**5. `[Phone]`:** Validates that the property value is a valid phone number.

```csharp
[Phone]
public string PhoneNumber { get; set; }
```

**6. `[RegularExpression]`:** Validates that the property value matches a specified regular expression.

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z]*$")]
public string Category { get; set; }
```

**7. `[Compare]`:** Validates that the property value matches the value of another property.

```csharp
[Compare("Password")]
public string ConfirmPassword { get; set; }
```

**8. `[Url]`:** Validates that the property value is a valid URL.

```csharp
[Url]
public string Website { get; set; }
```

**9. `[CreditCard]`:** Validates that the property value is a valid credit card number.

```csharp
[CreditCard]
public string CreditCardNumber { get; set; }
```

---

## Combine Model Validation and Tag Helpers

Combining Model Validation with Tag Helpers in ASP.NET Core allows you to create a form with automatic validation for your inputs. This approach provides a seamless integration of server-side validation with client-side rendering and feedback. Let's go through a step-by-step example to demonstrate this.

1. First, create a model class with validation attributes:

```csharp
public class RegisterViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [StringLength(100, MinimumLength = 6)]
    public string Password { get; set; }

    [Required]
    [Compare("Password")]
    public string ConfirmPassword { get; set; }
}
```

2. In your controller, create an action that returns a view for the form:

```csharp
public IActionResult Register()
{
    return View();
}
```

3. Create a corresponding view, and use Tag Helpers along with the `asp-validation-*` attributes to generate the necessary input elements, validation messages, and client-side validation scripts:

```html
@model RegisterViewModel

<form asp-action="Register" method="post">
    <div>
        <label asp-for="Email"></label>
        <input asp-for="Email" />
        <span asp-validation-for="Email"></span>
    </div>
    <div>
        <label asp-for="Password"></label>
        <input asp-for="Password" />
        <span asp-validation-for="Password"></span>
    </div>
    <div>
        <label asp-for="ConfirmPassword"></label>
        <input asp-for="ConfirmPassword" />
        <span asp-validation-for="ConfirmPassword"></span>
    </div>
    <button type="submit">Register</button>
</form>
```

The `asp-validation-for` attribute generates a `<span>` element that displays validation messages for the specified property. This works in conjunction with the `asp-for` attribute on the input element, which generates the necessary `name` and `id` attributes for the input and automatically binds it to the specified model property.

4. In your `_Layout.cshtml` or your specific view, include the necessary client-side validation libraries. Make sure you have these scripts in the correct order:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.19.3/jquery.validate.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validation-unobtrusive/3.2.12/jquery.validate.unobtrusive.min.js"></script>
```

5. Finally, in your controller, create a POST action to handle the form submission and perform server-side validation:

```csharp
[HttpPost]
public IActionResult Register(RegisterViewModel model)
{
    if (!ModelState.IsValid)
    {
        return View(model);
    }

    // Process registration and redirect to another action
}
```

In this step, when you define the `RegisterViewModel model` parameter in the POST action, ASP.NET Core **automatically uses Model Binding to bind the form data to the parameter**. You don't need to explicitly specify anything, as the framework takes care of this for you.

ASP.NET Core's Model Binding system automatically binds form data, route values, and query strings to action method parameters based on the parameter's name and type. In this case, since the form field names generated by the Tag Helpers match the property names in the `RegisterViewModel`, the framework can automatically bind the form data to the `model` parameter when the form is submitted.


By combining Model Validation with Tag Helpers in this manner, you can create a form with automatic validation for your inputs. The form will display validation messages based on the data annotations in your model, and perform client-side validation using the included jQuery Validate and jQuery Validation Unobtrusive libraries.

---

In summary, model binding and model validation are essential features of ASP.NET Core that simplify working with data in your web applications. Model binding automatically maps incoming request data to your action parameters, while model validation ensures that the data conforms to specified rules before processing.
