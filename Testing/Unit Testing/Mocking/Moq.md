# Moq

Moq is a popular, lightweight, and easy-to-use mocking library for .NET applications. It provides a simple and expressive API for creating mock objects, setting up their behavior, and verifying interactions between the code under test and its dependencies. Moq is particularly well-suited for mocking interfaces and abstract classes.

https://github.com/moq/moq

## Key Concepts

1. Creating mock objects: Moq allows you to create mock objects for interfaces or abstract classes using the `Mock<T>` class, where `T` is the type you want to mock.

```csharp
var mockRepository = new Mock<ICustomerRepository>();
```

2. Setting up behavior: You can define the behavior of a mock object using the `Setup` method. This allows you to specify the return values or actions for its methods or properties.

```csharp
mockRepository.Setup(repo => repo.GetCustomerById(1)).Returns(new Customer { Id = 1, FirstName = "John", LastName = "Doe" });
```

3. Verifying interactions: Moq enables you to verify that the expected methods were called on the mock object with the right arguments and the correct number of times using the `Verify` method.

```csharp
mockRepository.Verify(repo => repo.GetCustomerById(1), Times.Once);
```

4. Loose vs. Strict Mocking: Moq supports two mocking modes: loose and strict. In loose mode (the default), any unexpected method call on the mock object returns a default value. In strict mode, unexpected method calls throw an exception. You can set the mocking mode when creating the mock object.

```csharp
var strictMockRepository = new Mock<ICustomerRepository>(MockBehavior.Strict);
```

5. Callbacks and sequences: Moq allows you to set up more advanced behavior for your mocks, such as executing callbacks when a method is called or specifying a sequence of return values for successive calls.

```csharp
int callCount = 0;
mockRepository.Setup(repo => repo.GetCustomerById(It.IsAny<int>()))
    .Callback(() => callCount++)
    .Returns(new Customer { Id = 1, FirstName = "John", LastName = "Doe" });

mockRepository.SetupSequence(repo => repo.GetCustomerCount())
    .Returns(10)
    .Returns(20);
```

6. Argument matching: Moq provides argument matchers, like `It.IsAny<T>()`, `It.Is<T>(predicate)`, and `It.IsIn<T>(value)`, which allow you to set up behavior or verify method calls based on argument patterns rather than specific values.

```csharp
mockRepository.Setup(repo => repo.GetCustomerById(It.IsAny<int>())).Returns((int id) => new Customer { Id = id, FirstName = "John", LastName = "Doe" });

mockRepository.Verify(repo => repo.GetCustomerById(It.Is<int>(id => id > 0)), Times.Once);
```

## Examples

Let's suppose we have the following code and we want to write some tests:

```csharp
public interface ICustomerRepository
{
    Customer GetCustomerById(int id);
}

public class CustomerService
{
    private readonly ICustomerRepository _repository;

    public CustomerService(ICustomerRepository repository)
    {
        _repository = repository;
    }

    public string GetCustomerFullName(int id)
    {
        var customer = _repository.GetCustomerById(id);
        return customer != null ? $"{customer.FirstName} {customer.LastName}" : string.Empty;
    }
}
```

Let's look at an example using Moq to test the `CustomerService` class:

```csharp
using Moq;
using Xunit;

public class CustomerServiceTests
{
    [Fact]
    public void GetCustomerFullName_CustomerExists_ShouldReturnFullName()
    {
        // Arrange
        var mockRepository = new Mock<ICustomerRepository>();
        mockRepository.Setup(repo => repo.GetCustomerById(1)).Returns(new Customer { Id = 1, FirstName = "John", LastName = "Doe" });

        var customerService = new CustomerService(mockRepository.Object);

        // Act
        string fullName = customerService.GetCustomerFullName(1);

        // Assert
        Assert.Equal("John Doe", fullName);
        mockRepository.Verify(repo => repo.GetCustomerById(1), Times.Once);
    }
}
```
---

Let's consider a scenario where the `ICustomerRepository` interface has a method that could throw an exception:

```csharp
public interface ICustomerRepository
{
    Customer GetCustomerById(int id);
    void DeleteCustomer(int id);
}
```

Suppose the `CustomerService` has a method to delete a customer:

```csharp
public class CustomerService
{
    // ...

    public bool TryDeleteCustomer(int id)
    {
        try
        {
            _repository.DeleteCustomer(id);
            return true;
        }
        catch (Exception)
        {
            return false;
        }
    }
}
```

We can use Moq to test the `TryDeleteCustomer` method and verify that it handles exceptions correctly:

```csharp
using Moq;
using Xunit;

public class CustomerServiceTests
{
    // ...

    [Fact]
    public void TryDeleteCustomer_ExceptionThrown_ShouldReturnFalse()
    {
        // Arrange
        var mockRepository = new Mock<ICustomerRepository>();
        mockRepository.Setup(repo => repo.DeleteCustomer(1)).Throws(new InvalidOperationException("Unable to delete customer."));

        var customerService = new CustomerService(mockRepository.Object);

        // Act
        bool result = customerService.TryDeleteCustomer(1);

        // Assert
        Assert.False(result);
        mockRepository.Verify(repo => repo.DeleteCustomer(1), Times.Once);
    }
}
```

In this example, we set up the mock repository to throw an exception when the `DeleteCustomer` method is called with the argument `1`. Then, we test the `TryDeleteCustomer` method of `CustomerService` and verify that it returns `false` when an exception is thrown, indicating that it handles exceptions correctly.

---

In summary, Moq is a powerful and flexible mocking library for .NET that provides a simple and expressive API for creating mock objects, setting up their behavior, and verifying interactions between the code under test and its dependencies. The examples provided demonstrate various aspects of Moq, such as setting up behavior, verifying interactions, handling exceptions, and using argument matchers. In the next prompt, I will explain NSubstitute, another popular mocking library for .NET, and provide examples for comparison.