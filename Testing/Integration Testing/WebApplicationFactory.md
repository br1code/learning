# WebApplicationFactory

WebApplicationFactory is a class provided by the ASP.NET Core Test Host package that enables integration testing for ASP.NET Core applications. It allows you to create an instance of your web application, including its dependency injection container, hosting environment, and other configurations, all within the context of a test environment.

Here's a real-world example of integration testing using WebApplicationFactory for an e-commerce application. The application has the following components:

- An `OrdersController` that handles placing orders and retrieving order details.
- An `IOrdersService` that contains the business logic for processing orders.
- An `IOrdersRepository` that interacts with a data store to store and retrieve order information.

First, create an `IntegrationTests` project, and add a reference to the `Microsoft.AspNetCore.Mvc.Testing` NuGet package. Then, create a `CustomWebApplicationFactory` class that inherits from `WebApplicationFactory<T>`:

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc.Testing;
using Microsoft.AspNetCore.TestHost;
using Microsoft.Extensions.DependencyInjection;
using MyECommerceApp;

public class CustomWebApplicationFactory<TStartup> : WebApplicationFactory<TStartup>
    where TStartup : class
{
    protected override void ConfigureWebHost(IWebHostBuilder builder)
    {
        builder.ConfigureServices(services =>
        {
            // Register test-specific services, mocks, or other configurations here.
            // For example, replace a real repository implementation with a test implementation.
            services.AddScoped<IOrdersRepository, TestOrdersRepository>();
        });
    }
}
```

Next, create a test class that uses the `CustomWebApplicationFactory` to perform integration tests on the `OrdersController`. The test will create a new order and then retrieve its details:

```csharp
using System.Net.Http;
using System.Net.Http.Json;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc.Testing;
using MyECommerceApp;
using Xunit;

public class OrdersControllerTests : IClassFixture<CustomWebApplicationFactory<MyECommerceApp.Startup>>
{
    private readonly HttpClient _client;

    public OrdersControllerTests(CustomWebApplicationFactory<MyECommerceApp.Startup> factory)
    {
        _client = factory.CreateClient();
    }

    [Fact]
    public async Task PlaceOrderAndRetrieveOrderDetails_Success()
    {
        // Arrange
        var newOrder = new Order
        {
            CustomerId = 1,
            ProductId = 101,
            Quantity = 2
        };

        // Act: Place the order
        var response = await _client.PostAsJsonAsync("/api/orders", newOrder);
        response.EnsureSuccessStatusCode();
        var createdOrder = await response.Content.ReadFromJsonAsync<Order>();

        // Assert: Check if the order was created successfully
        Assert.NotNull(createdOrder);
        Assert.NotEqual(0, createdOrder.Id);

        // Act: Retrieve the order details
        response = await _client.GetAsync($"/api/orders/{createdOrder.Id}");
        response.EnsureSuccessStatusCode();
        var retrievedOrder = await response.Content.ReadFromJsonAsync<Order>();

        // Assert: Check if the retrieved order matches the created order
        Assert.NotNull(retrievedOrder);
        Assert.Equal(createdOrder.Id, retrievedOrder.Id);
        Assert.Equal(newOrder.CustomerId, retrievedOrder.CustomerId);
        Assert.Equal(newOrder.ProductId, retrievedOrder.ProductId);
        Assert.Equal(newOrder.Quantity, retrievedOrder.Quantity);
    }
}
```

In this example, the `CustomWebApplicationFactory` creates a test environment for the e-commerce application, and the `OrdersControllerTests` class performs an integration test that involves placing an order and retrieving its details. The `PlaceOrderAndRetrieveOrderDetails_Success` test method first posts a new order to the `/api/orders` endpoint, then retrieves the order details from the `/api/orders/{orderId}` endpoint, and finally asserts that the retrieved order matches the created order.

Here's a brief explanation of the key components in the test:

- `CustomWebApplicationFactory`: This class is responsible for creating and configuring the test environment for the web application. In this example, it replaces the `IOrdersRepository` implementation with a test-specific implementation, `TestOrdersRepository`. You can also configure other services or settings as needed for your tests.

- `OrdersControllerTests`: This class contains the integration tests for the `OrdersController`. It uses the `CustomWebApplicationFactory` to create an `HttpClient` instance for making HTTP requests to the application. The `IClassFixture` interface ensures that the `CustomWebApplicationFactory` is shared across all tests within the class, improving performance and reducing resource usage.

- `PlaceOrderAndRetrieveOrderDetails_Success`: This is an individual test method that performs the integration test scenario. It uses the `_client` instance to send HTTP requests to the application and checks the responses to ensure that the application is behaving as expected.

This example demonstrates how to use the `WebApplicationFactory` class in .NET to perform integration testing for an ASP.NET Core web application. By using `WebApplicationFactory`, you can create realistic test environments for your application and easily test its behavior and interactions with various components, such as controllers, services, and data repositories.