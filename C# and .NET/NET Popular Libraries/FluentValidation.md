# FluentValidation

https://www.nuget.org/packages/fluentvalidation/

## Simple Explanation

FluentValidation is a popular validation library for .NET that allows you to define validation rules for your models in a fluent and expressive way. It is designed to be easy to use, flexible, and extensible, and provides a wide range of built-in validation rules as well as the ability to create your own custom rules.

## Deep Explanation

FluentValidation is a .NET library that provides a fluent interface for defining validation rules for your models. It is designed to be used with any object that implements the IValidatableObject interface or has a public parameterless constructor. The library allows you to define your validation rules in a fluent and expressive way, making it easy to read and understand your code.

One of the key features of FluentValidation is its ability to create complex validation rules that involve multiple properties of your models. For example, you can define a rule that requires one property to be greater than another property, or that requires two properties to have the same value. This can be done using a fluent syntax that is both concise and easy to read.

FluentValidation provides a wide range of built-in validation rules, including rules for validating strings, numbers, dates, and collections, as well as rules for validating email addresses, URLs, and regular expressions. You can also create your own custom validation rules by inheriting from the AbstractValidator<T> class and defining your own validation logic.

In addition to its core functionality, FluentValidation also provides a number of additional features, such as support for localization, conditional validation, and client-side validation in web applications.

## Examples

Here's a simple example of how to use FluentValidation to validate a Person object:

```C#
public class PersonValidator : AbstractValidator<Person>
{
    public PersonValidator()
    {
        RuleFor(p => p.FirstName).NotEmpty().WithMessage("Please enter a first name.");
        RuleFor(p => p.LastName).NotEmpty().WithMessage("Please enter a last name.");
        RuleFor(p => p.Email).NotEmpty().EmailAddress().WithMessage("Please enter a valid email address.");
    }
}

public class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
}

// Usage
var person = new Person { FirstName = "John", LastName = "Doe", Email = "johndoe@example.com" };
var validator = new PersonValidator();
var result = validator.Validate(person);

if (result.IsValid)
{
    // Object is valid
}
else
{
    foreach (var error in result.Errors)
    {
        Console.WriteLine(error.ErrorMessage);
    }
}
```

---

Here's an example of using FluentValidation with xUnit in a .NET Core application:

Suppose we have a Person class with two properties FirstName and LastName, and we want to ensure that FirstName and LastName are not empty or null. Here's how we can create a PersonValidator class using FluentValidation to validate Person objects:

```C#
using FluentValidation;

public class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
}

public class PersonValidator : AbstractValidator<Person>
{
    public PersonValidator()
    {
        RuleFor(person => person.FirstName)
            .NotEmpty().WithMessage("Please enter a first name.");

        RuleFor(person => person.LastName)
            .NotEmpty().WithMessage("Please enter a last name.");
    }
}
```

We can then use this validator in our unit tests as follows:

```C#
using FluentValidation.TestHelper;
using Xunit;

public class PersonValidatorTests
{
    private readonly PersonValidator _validator;

    public PersonValidatorTests()
    {
        _validator = new PersonValidator();
    }

    [Fact]
    public void FirstName_Should_Not_Be_Null_Or_Empty()
    {
        // Arrange
        var person = new Person { FirstName = null, LastName = "Doe" };

        // Act
        var result = _validator.TestValidate(person);

        // Assert
        result.ShouldHaveValidationErrorFor(p => p.FirstName);
    }

    [Fact]
    public void LastName_Should_Not_Be_Null_Or_Empty()
    {
        // Arrange
        var person = new Person { FirstName = "John", LastName = "" };

        // Act
        var result = _validator.TestValidate(person);

        // Assert
        result.ShouldHaveValidationErrorFor(p => p.LastName);
    }

    [Fact]
    public void FirstName_And_LastName_Should_Be_Valid()
    {
        // Arrange
        var person = new Person { FirstName = "John", LastName = "Doe" };

        // Act
        var result = _validator.TestValidate(person);

        // Assert
        result.ShouldNotHaveAnyValidationErrorFor(p => p.FirstName);
        result.ShouldNotHaveAnyValidationErrorFor(p => p.LastName);
    }
}
```