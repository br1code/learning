# Fundamentals of Mocking

Mocking is a technique used in unit testing to create substitute objects, called "mocks," that simulate the behavior of real objects or components in a controlled manner. Mocks are used to isolate the code under test from external dependencies, such as databases, web services, or other components, making the tests more focused, reliable, and easier to maintain.

Mocking is especially useful when working with complex systems or components that are difficult to set up, have side effects, or are slow to execute. By replacing these dependencies with mocks, you can simplify your tests and ensure they run quickly and consistently.

Here are some key concepts related to mocking:

1. Mock objects: Mock objects are instances of classes or interfaces that simulate the behavior of real objects or components. They can be programmed to return specific values, throw exceptions, or perform actions when their methods are called.

2. Mocking frameworks: Mocking frameworks are libraries that provide tools and utilities to create, configure, and interact with mock objects. Some popular mocking frameworks for .NET include Moq, NSubstitute, Rhino Mocks, and FakeItEasy.

3. Stubbing: Stubbing is the process of defining the behavior of a mock object, such as specifying the return values for its methods or properties. Stubs are typically used when you want to replace a real object with a simple, predictable substitute that doesn't affect the outcome of the test.

4. Mocking: Mocking, in the context of mock objects, involves not only stubbing the behavior of the object but also verifying that its methods were called with the expected arguments and the correct number of times. This allows you to test the interactions between the code under test and its dependencies.

5. Test doubles: Test doubles are a more general term that encompasses all types of substitute objects used in testing, including mocks, stubs, dummies, fakes, and spies.

To illustrate mocking in practice, let's consider an example. Imagine you have an `ICustomerRepository` interface for accessing customer data and a `CustomerService` class that depends on it:

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

To test the `GetCustomerFullName` method of `CustomerService`, you can create a mock of the `ICustomerRepository` interface, stub its `GetCustomerById` method to return a specific customer, and then verify that the `GetCustomerFullName` method returns the correct full name.

## Test doubles in depth

Test doubles are substitute objects used in unit testing to replace real dependencies and isolate the code under test. There are five main types of test doubles: mocks, stubs, dummies, fakes, and spies. Each type serves a different purpose and has its own characteristics. Here's a brief explanation of each type and examples using the Moq framework.

1. Mocks:
Mocks are objects that simulate the behavior of real objects and can be programmed to return specific values, throw exceptions, or perform actions when their methods are called. They also record the interactions between the code under test and the mock, allowing you to verify that the expected methods were called with the right arguments and the correct number of times.

Example:

```csharp
var mockRepository = new Mock<ICustomerRepository>();
mockRepository.Setup(repo => repo.GetCustomerById(1)).Returns(new Customer { Id = 1, FirstName = "John", LastName = "Doe" });

var customerService = new CustomerService(mockRepository.Object);
string fullName = customerService.GetCustomerFullName(1);

mockRepository.Verify(repo => repo.GetCustomerById(1), Times.Once);
```

2. Stubs:
Stubs are objects that provide predetermined responses to method calls or property access. They are typically used when you want to replace a real object with a simple, predictable substitute that doesn't affect the outcome of the test. Stubs don't record interactions or allow you to verify method calls.

Example:

```csharp
var stubRepository = new Mock<ICustomerRepository>();
stubRepository.Setup(repo => repo.GetCustomerById(1)).Returns(new Customer { Id = 1, FirstName = "John", LastName = "Doe" });

var customerService = new CustomerService(stubRepository.Object);
string fullName = customerService.GetCustomerFullName(1);
```

3. Dummies:
Dummies are objects that are passed to the code under test but never actually used. They are often required to satisfy method signatures or constructor parameters but have no impact on the test's behavior. Dummy objects can be created using the default constructor or by assigning default values to their properties.

Example:

```csharp
public class DummyLogger : ILogger
{
    public void Log(string message) { }
}

var dummyLogger = new DummyLogger();
var customerService = new CustomerService(new Mock<ICustomerRepository>().Object, dummyLogger);
```

4. Fakes:
Fakes are objects that have a working implementation but usually take shortcuts to make them simpler or faster than the real objects they replace. Fakes can be used to replace complex dependencies, such as databases or web services, with lightweight in-memory implementations that are easier to set up and manage in tests.

Example:

```csharp
public class FakeCustomerRepository : ICustomerRepository
{
    private readonly Dictionary<int, Customer> _customers = new Dictionary<int, Customer>();

    public void AddCustomer(Customer customer)
    {
        _customers[customer.Id] = customer;
    }

    public Customer GetCustomerById(int id)
    {
        _customers.TryGetValue(id, out var customer);
        return customer;
    }
}

var fakeRepository = new FakeCustomerRepository();
fakeRepository.AddCustomer(new Customer { Id = 1, FirstName = "John", LastName = "Doe" });

var customerService = new CustomerService(fakeRepository);
string fullName = customerService.GetCustomerFullName(1);
```

5. Spies:
Spies are objects that wrap around real objects and record the method calls made to them. They can be used to verify interactions with the real object while still allowing the real object to perform its normal operations.

Example:

```csharp
public class SpyCustomerRepository : ICustomerRepository
{
    private readonly ICustomerRepository _innerRepository;
    private int _getCustomerByIdCallCount;

    public SpyCustomerRepository(ICustomerRepository innerRepository)
    {
        _innerRepository = innerRepository;
        _getCustomerByIdCallCount = 0;
    }

    public Customer GetCustomerById(int id)
    {
        _getCustomerByIdCallCount++;
        return _innerRepository.GetCustomerById(id);
    }

    public int GetCustomerByIdCallCount => _getCustomerByIdCallCount;
}

var realRepository = new RealCustomerRepository();
var spyRepository = new SpyCustomerRepository(realRepository);
var customerService = new CustomerService(spyRepository);
string fullName = customerService.GetCustomerFullName(1);

Assert.AreEqual(1, spyRepository.GetCustomerByIdCallCount);
```

In this example, we create a `SpyCustomerRepository` class that wraps around a real `ICustomerRepository` implementation. It records the number of times the `GetCustomerById` method is called and exposes this information through the `GetCustomerByIdCallCount` property. This allows us to verify the interactions with the real repository while still executing the real implementation.

To summarize, test doubles are substitute objects used in unit testing to replace real dependencies and isolate the code under test. Each type of test double serves a different purpose:

- Mocks: Verify interactions and can be programmed to return specific values or behaviors.
- Stubs: Provide predetermined responses without verifying interactions.
- Dummies: Satisfy method signatures or constructor parameters without affecting test behavior.
- Fakes: Offer simplified or lightweight implementations of real objects.
- Spies: Record method calls made to real objects while allowing the real object to perform its normal operations.

Using test doubles appropriately can help you create more focused, reliable, and maintainable unit tests. In the examples provided, we used the Moq framework to create mocks and stubs, but the concepts apply to other mocking frameworks as well.