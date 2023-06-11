# MediatR

https://www.nuget.org/packages/MediatR

## Simple Explanation

MediatR is a popular package in C# and .NET that enables the implementation of the mediator pattern, which is a design pattern used for loosely-coupled communication between different components of an application. It simplifies the implementation of request/response messaging patterns between different parts of an application by eliminating the need for direct coupling between the components.

## Deep Explanation

The MediatR package in C# and .NET is a powerful tool for implementing the mediator pattern in an application. The mediator pattern is a design pattern that decouples the components of an application by enabling communication between them through a mediator instead of direct coupling. This design pattern makes the application more flexible and easier to maintain and extend over time.

The MediatR package enables developers to implement this pattern by providing a mediator implementation that handles communication between components. The package allows developers to define and send messages between different parts of an application without having to deal with the specifics of how each message is handled. This makes it easy to implement complex request/response messaging patterns in an application without having to worry about the underlying implementation details.

The core concepts in MediatR are:

- Request: A request is an object that represents a message sent from one component of an application to another component.

- Handler: A handler is a class that contains the logic for processing a request.

- Mediator: A mediator is an object that manages the communication between different components of an application.

- Pipeline: A pipeline is a series of steps that a request goes through before being handled by a handler. These steps can be used for tasks such as validation, logging, and authentication.

By using MediatR, developers can easily implement the mediator pattern in their applications, making them more flexible and easier to maintain. They can also take advantage of features such as pipelines to add additional functionality to their request processing logic.

## Examples

Here's an example of how to use MediatR in a simple C# console application:

```C#
public class MyRequest : IRequest<string>
{
    public string Input { get; set; }
}

public class MyRequestHandler : IRequestHandler<MyRequest, string>
{
    public Task<string> Handle(MyRequest request, CancellationToken cancellationToken)
    {
        return Task.FromResult($"Hello, {request.Input}!");
    }
}
```

In this example, the MyRequest class represents a message that will be sent between components of the application. The MyRequestHandler class contains the logic for processing this request.

Finally, you can use MediatR to send the request and get the response:

```C#
var mediator = new Mediator(new ServiceFactory(type => container.GetInstance(type)));

var response = await mediator.Send(new MyRequest { Input = "World" });

Console.WriteLine(response);
```

---

Here's a basic example of how to implement MediatR in an ASP.NET Web API application:

Create a new command that you want to handle using MediatR. For example:

```C#
public class CreateProductCommand : IRequest<int>
{
    public string Name { get; set; }
    public decimal Price { get; set; }
}
```

Create a handler for the command. For example:

```C#
public class CreateProductCommandHandler : IRequestHandler<CreateProductCommand, int>
{
    private readonly IDbContext _dbContext;

    public CreateProductCommandHandler(IDbContext dbContext)
    {
        _dbContext = dbContext;
    }

    public async Task<int> Handle(CreateProductCommand request, CancellationToken cancellationToken)
    {
        var product = new Product
        {
            Name = request.Name,
            Price = request.Price
        };

        await _dbContext.Products.AddAsync(product, cancellationToken);
        await _dbContext.SaveChangesAsync(cancellationToken);

        return product.Id;
    }
}
```

Create a new query that you want to handle using MediatR. For example:

```C#
public class GetProductsQuery : IRequest<List<Product>>
{
    public int MaxPrice { get; set; }
}
```

Create a handler for the query:

```C#
public class GetProductsQueryHandler : IRequestHandler<GetProductsQuery, List<Product>>
{
    private readonly MyDbContext _dbContext;

    public GetProductsQueryHandler(MyDbContext dbContext)
    {
        _dbContext = dbContext;
    }

    public Task<List<Product>> Handle(GetProductsQuery request, CancellationToken cancellationToken)
    {
        return _dbContext.Products.Where(p => p.Price <= request.MaxPrice).ToListAsync(cancellationToken);
    }
}
```

In the ConfigureServices method in Startup.cs, register the MediatR service and any handlers:

```C#
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();

    services.AddMediatR(cfg => {
        cfg.RegisterServicesFromAssembly(typeof(Program).Assembly);
    });
}
```

This method will register the known MediatR types (such as IMediator) as transient, and for each assembly registered, it will scan those assemblies and register concrete implementations of types (such as IRequestHandler) as transient.

In your controller, inject an instance of IMediator and use it to send a command or query:

```C#
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    private readonly IMediator _mediator;

    public ProductsController(IMediator mediator)
    {
        _mediator = mediator;
    }

    [HttpPost]
    public async Task<IActionResult> CreateProduct(CreateProductCommand command)
    {
        var productId = await _mediator.Send(command);

        return Ok(productId);
    }

    [HttpGet]
    public async Task<IActionResult> GetProducts(int maxPrice)
    {
        var query = new GetProductsQuery { MaxPrice = maxPrice };
        var products = await _mediator.Send(query);

        return Ok(products);
    }
}
```