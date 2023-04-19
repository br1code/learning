# Dependency Injection Libraries

## Microsoft.Extensions.DependencyInjection

### Explanation

Microsoft.Extensions.DependencyInjection is a library that provides a lightweight and flexible dependency injection framework for .NET applications. It allows you to easily register and resolve dependencies, manage object lifetimes, and control how dependencies are created and consumed in your code.

In Microsoft.Extensions.DependencyInjection, dependencies are registered using the IServiceCollection interface, which allows you to specify the type of the dependency and how it should be created. 

### Examples

For example, you can register a singleton instance of a class using the following code:

```C#
services.AddSingleton<MyService>();
```

You can also register a type with a specific interface:

```C#
services.AddSingleton<IMyService, MyService>();
```

Once dependencies are registered, they can be resolved using the IServiceProvider interface. This interface provides methods for resolving single instances of dependencies or collections of dependencies. For example, you can resolve a single instance of MyService using the following code:

```C#
var myService = serviceProvider.GetService<MyService>();
```

You can also resolve a collection of services using the following code:

```C#
var myServices = serviceProvider.GetServices<IMyService>();
```

In addition to resolving dependencies, Microsoft.Extensions.DependencyInjection also supports a number of advanced features, such as conditional registration, configuration binding, and interception.

Here's another example that demonstrates how to register and resolve a type with a specific interface:

```C#
public interface IMyService
{
    void DoSomething();
}

public class MyService : IMyService
{
    public void DoSomething()
    {
        Console.WriteLine("Doing something...");
    }
}

// In your application startup code:
var services = new ServiceCollection();
services.AddScoped<IMyService, MyService>();
var serviceProvider = services.BuildServiceProvider();

// In your application code:
var myService = serviceProvider.GetService<IMyService>();
myService.DoSomething();
```

## Scrutor

### Explanation

Scrutor is a library in C# and .NET that provides an easier way to scan and configure dependencies in a project. It simplifies the registration of services by allowing you to use attributes to indicate how services should be registered.

In a larger project, dependency injection can become difficult to manage. You might have to register many services in many different parts of your codebase. Scrutor simplifies this process by providing a fluent interface that allows you to **scan assemblies and register services based on attributes**.

Scrutor provides a number of extension methods that make it easy to register services. These methods allow you to register services by convention, rather than explicitly. For example, you can use the AddClasses method to register all classes in an assembly that implement a certain interface.

Scrutor also provides a number of attributes that you can use to specify how services should be registered. For example, you can use the ServiceDescriptorAttribute to specify a service should be registered as a singleton, transient, or scoped service. This makes it easy to manage the lifetime of services.

### Examples

1 - Register all services in an assembly that implement a certain interface:

```C#
services.Scan(scan => scan
    .FromAssemblyOf<IMyService>()
    .AddClasses(classes => classes.AssignableTo<IMyService>())
    .AsImplementedInterfaces()
    .WithScopedLifetime());
```

This code scans the assembly that contains IMyService and registers all classes that implement IMyService as scoped services that are resolved by their implemented interfaces.

2 - Register all services in an assembly that have a certain attribute:

```C#
services.Scan(scan => scan
    .FromAssemblyOf<IMyService>()
    .AddClasses(classes => classes.WithAttribute<MyAttribute>())
    .AsSelf()
    .WithTransientLifetime());
```

This code scans the assembly that contains IMyService and registers all classes that have the MyAttribute attribute as transient services that are resolved by their own type.

3 - Use the ServiceDescriptorAttribute to specify the lifetime of a service:

```C#
[ServiceDescriptor(ServiceLifetime.Singleton)]
public class MySingletonService : IMyService
{
    // ...
}

services.Scan(scan => scan
    .FromAssemblyOf<IMyService>()
    .AddClasses(classes => classes.AssignableTo<IMyService>())
    .AsImplementedInterfaces()
    .WithServiceDescriptor());
```

This code registers all classes that implement IMyService as services and uses the ServiceDescriptorAttribute to specify that MySingletonService should be registered as a singleton service.