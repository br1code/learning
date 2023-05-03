# NSubstitute

NSubstitute is a popular, easy-to-use mocking library for .NET applications. It provides a simple and expressive API for creating substitute objects, configuring their behavior, and verifying interactions between the code under test and its dependencies. NSubstitute is designed to be friendly and intuitive, which makes it a great choice for developers new to mocking or looking for a more straightforward API.

https://nsubstitute.github.io/

## Key Concepts

1. Creating substitute objects: NSubstitute allows you to create substitute objects for interfaces or classes (including non-abstract classes with virtual members) using the `Substitute.For<T>` method, where `T` is the type you want to substitute.

```csharp
var repository = Substitute.For<ICustomerRepository>();
```

2. Setting up behavior: You can define the behavior of a substitute object by assigning values or actions to its methods or properties using the `Returns` or `ReturnsForAnyArgs` methods.

```csharp
repository.GetCustomerById(1).Returns(new Customer { Id = 1, FirstName = "John", LastName = "Doe" });
```

3. Verifying interactions: NSubstitute enables you to verify that the expected methods were called on the substitute object with the right arguments and the correct number of times using the `Received` method.

```csharp
repository.Received().GetCustomerById(1);
```

4. Argument matching: NSubstitute provides argument matchers, like `Arg.Any<T>()`, `Arg.Is<T>(predicate)`, and `Arg.Invoke<T>(value)`, which allow you to set up behavior or verify method calls based on argument patterns rather than specific values.

```csharp
repository.GetCustomerById(Arg.Any<int>()).Returns((int id) => new Customer { Id = id, FirstName = "John", LastName = "Doe" });

repository.Received().GetCustomerById(Arg.Is<int>(id => id > 0));
```

5. Callbacks and sequences: NSubstitute allows you to set up more advanced behavior for your substitutes, such as executing callbacks when a method is called or specifying a sequence of return values for successive calls.

```csharp
int callCount = 0;
repository.GetCustomerById(Arg.Any<int>()).Returns((int id) => new Customer { Id = id, FirstName = "John", LastName = "Doe" })
                                           .AndDoes(_ => callCount++);

repository.GetCustomerCount().Returns(10, 20);
```

6. Checking received calls: NSubstitute provides the `Received.InOrder` method, which allows you to verify that a series of calls were made in a specific order.

```csharp
Received.InOrder(() =>
{
    repository.GetCustomerById(1);
    repository.DeleteCustomer(1);
});
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

Let's look at an example using NSubstitute to test the `CustomerService` class:

```csharp
using NSubstitute;
using Xunit;

public class CustomerServiceTests
{
    [Fact]
    public void GetCustomerFullName_CustomerExists_ShouldReturnFullName()
    {
        // Arrange
        var repository = Substitute.For<ICustomerRepository>();
        repository.GetCustomerById(1).Returns(new Customer { Id = 1, FirstName = "John", LastName = "Doe" });

        var customerService = new CustomerService(repository);

        // Act
        string fullName = customerService.GetCustomerFullName(1);

        // Assert
        Assert.Equal("John Doe", fullName);
        repository.Received().GetCustomerById(1);
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

We can use NSubstitute to test the `TryDeleteCustomer` method and verify that it handles exceptions correctly:

```csharp
using NSubstitute;
using Xunit;

public class CustomerServiceTests
{
    // ...

    [Fact]
    public void TryDeleteCustomer_ExceptionThrown_ShouldReturnFalse()
    {
        // Arrange
        var repository = Substitute.For<ICustomerRepository>();
        repository.DeleteCustomer(1).Throws(new InvalidOperationException("Unable to delete customer."));

        var customerService = new CustomerService(repository);

        // Act
        bool result = customerService.TryDeleteCustomer(1);

        // Assert
        Assert.False(result);
        repository.Received().DeleteCustomer(1);
    }
}
```

In this example, we set up the substitute repository to throw an exception when the `DeleteCustomer` method is called with the argument `1`. Then, we test the `TryDeleteCustomer` method of `CustomerService` and verify that it returns `false` when an exception is thrown, indicating that it handles exceptions correctly.

This example demonstrates how to handle exceptions using NSubstitute, which is similar to Moq, but with a more concise syntax.

---

In summary, NSubstitute is a powerful and user-friendly mocking library for .NET that provides a simple and expressive API for creating substitute objects, setting up their behavior, and verifying interactions between the