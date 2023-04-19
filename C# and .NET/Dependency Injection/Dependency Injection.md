# Dependency Injection

## Simple Explanation

Dependency Injection (DI) is a design pattern that helps to improve the modularity, flexibility, and testability of software applications. It involves decoupling objects by allowing them to request dependencies (other objects or values they need to work) from an external source instead of creating them directly. This way, the objects can be easily replaced, extended or tested without changing the code that uses them.

## Deep Explanation

Dependency Injection is based on the principle of Inversion of Control (IoC), which means that instead of a component controlling its dependencies, the dependencies are injected into the component by an external entity (often called a container). This way, the component doesn't need to know how to create or find its dependencies, and can focus on doing its job.

There are three main types of Dependency Injection:

- Constructor Injection: In this type of injection, the dependencies are passed as parameters to the constructor of the component. This way, the component knows exactly what it needs to work, and can't be instantiated without it. Constructor Injection is the **most common and recommended** type of injection.

- Property Injection: In this type of injection, the dependencies are set as properties of the component after it is created. This type of injection is less recommended as it can lead to null reference exceptions if the properties are not set properly.

- Method Injection: In this type of injection, the dependencies are passed as parameters to specific methods of the component when they are needed. This type of injection is less common and less recommended as it can lead to messy and hard to read code.

To implement Dependency Injection in C# and .NET, you can use a DI container (like Autofac, Ninject, or Microsoft's built-in container) that manages the creation and lifetime of objects and their dependencies. The container can be configured with mappings between interfaces and concrete implementations, and can automatically resolve the dependencies of objects that are created through it.

## Examples

```C#
public interface ILogger {
    void Log(string message);
}

public class ConsoleLogger : ILogger {
    public void Log(string message) {
        Console.WriteLine(message);
    }
}

public class MyClass {
    private ILogger _logger;

    public MyClass(ILogger logger) {
        _logger = logger;
    }

    public void DoSomething() {
        _logger.Log("Doing something...");
    }
}

// In the composition root of the application:
var builder = new Autofac.ContainerBuilder();
builder.RegisterType<ConsoleLogger>().As<ILogger>();
builder.RegisterType<MyClass>();
var container = builder.Build();

// In the code that uses MyClass:
var myClass = container.Resolve<MyClass>();
myClass.DoSomething();
```