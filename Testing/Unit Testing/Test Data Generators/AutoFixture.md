# AutoFixture

https://github.com/AutoFixture/AutoFixture

AutoFixture is a popular and powerful library for .NET that helps generate test data for your unit tests. It automatically creates objects with randomly generated values for their properties, making it easy to create test data without the need for manual setup. AutoFixture is highly customizable and can be used with various testing frameworks like xUnit, NUnit, and MSTest.

Here are some key concepts and features of AutoFixture:

1. Creating test data: AutoFixture provides the `Fixture` class, which is the primary object used to create test data. You can use the `Create<T>` method to generate an instance of a type with random values for its properties.

```csharp
var fixture = new Fixture();
var customer = fixture.Create<Customer>();
```

2. Customizing object creation: AutoFixture allows you to customize how objects are created by using the `Customize<T>` method on a `Fixture` instance. You can provide your own customization classes or lambda expressions to modify the object creation process.

```csharp
fixture.Customize<Customer>(c => c.With(x => x.FirstName, "John").With(x => x.LastName, "Doe"));
```

3. Generating collections: AutoFixture can generate collections of objects with the `CreateMany<T>` method, which generates a specified number of objects (default is 3) with random property values.

```csharp
var customers = fixture.CreateMany<Customer>(5);
```

4. Freezing and injecting dependencies: AutoFixture supports the concept of "freezing" an instance, which means that it will always return the same instance when creating objects of that type. This is useful for injecting dependencies in your tests.

```csharp
var repository = fixture.Freeze<ICustomerRepository>();
```

Note: This may not me available in latest versions.

5. Auto-mocking: AutoFixture can be integrated with mocking libraries like Moq and NSubstitute to automatically create mock objects and configure their behavior based on the provided customizations.

```csharp
var fixture = new Fixture().Customize(new AutoMoqCustomization());
var customerService = fixture.Create<CustomerService>();
```

6. Data attributes for test frameworks: AutoFixture provides data attributes for various testing frameworks, like `[AutoData]`, `[InlineAutoData]`, and `[AutoNSubstituteData]`, that can be used to automatically generate test data and pass it to your test methods.

```csharp
[Theory, AutoData]
public void GetCustomerFullName_CustomerExists_ShouldReturnFullName(Customer customer, ICustomerRepository repository)
{
    // ...
}
```

Let's see an example of using AutoFixture with xUnit to test the `CustomerService` class:

```csharp
using AutoFixture;
using AutoFixture.AutoMoq;
using AutoFixture.Xunit2;
using Moq;
using Xunit;

public class CustomerServiceTests
{
    [Theory, AutoData]
    public void GetCustomerFullName_CustomerExists_ShouldReturnFullName(
        Customer customer, [Frozen] Mock<ICustomerRepository> mockRepository, CustomerService customerService)
    {
        // Arrange
        mockRepository.Setup(repo => repo.GetCustomerById(customer.Id)).Returns(customer);

        // Act
        string fullName = customerService.GetCustomerFullName(customer.Id);

        // Assert
        Assert.Equal($"{customer.FirstName} {customer.LastName}", fullName);
        mockRepository.Verify(repo => repo.GetCustomerById(customer.Id), Times.Once);
    }
}
```

In this example, we use the `[AutoData]` attribute from AutoFixture to automatically generate test data and pass it to the test method. The `Customer` instance and the `Mock<ICustomerRepository>` instance are created and configured by AutoFixture. This simplifies the test code and reduces the need for manual setup.