# Composition over inheritance

## Simple Explanation

The "Composition over Inheritance" pattern is a software design principle that suggests favoring object composition over class inheritance. In other words, it's better to build software systems by composing smaller, reusable components instead of inheriting behaviors from existing classes. This approach promotes code reuse, enhances flexibility, and minimizes dependencies between classes.

## Deep Explanation

Inheritance allows a class to inherit properties and methods from its parent class, which can be useful for code reuse. However, inheritance can also lead to complex and rigid class hierarchies that are hard to modify and maintain. In contrast, composition allows building complex objects by combining simpler ones.

Composition means that a class contains an instance of another class as a member variable, and the composite object uses the services of the contained object to implement its behavior. This approach allows classes to be more modular, and to change their behavior by swapping out their composed components.

In essence, composition promotes loose coupling and separation of concerns, which makes the code more modular and easier to maintain. It also allows for greater flexibility and adaptability to change.

## Examples

Here's an example of using composition in C#:

```csharp
public class Engine
{
    public void Start()
    {
        Console.WriteLine("Engine started");
    }
}

public class Car
{
    private Engine _engine;

    public Car()
    {
        _engine = new Engine();
    }

    public void Start()
    {
        _engine.Start();
    }
}
```

In this example, the `Car` class contains an instance of the `Engine` class as a member variable. When the `Car.Start()` method is called, it invokes the `Engine.Start()` method to start the car's engine. This example demonstrates how composition can be used to build complex objects from simpler ones.

Another example of composition in .NET is the `System.Collections.Generic.List<T>` class, which is a collection that is composed of an array of elements. The `List<T>` class uses the services of the array to implement its behavior, such as adding and removing elements. This allows for greater flexibility in how the `List<T>` class can be used and modified.

Overall, using composition over inheritance promotes more flexible and modular code, which can lead to more maintainable and adaptable software systems.

---

Here's a real-world example of using composition over inheritance in a web application:

Consider a web application that allows users to sign up, log in, and manage their profiles. One way to implement this functionality is to create a `User` class that inherits from a `Person` class, which inherits from a `BaseEntity` class. The `BaseEntity` class might contain properties such as `Id`, `CreatedAt`, and `UpdatedAt`, which are common to all entities in the system.

However, this approach can create a rigid and complex class hierarchy that makes it difficult to modify or extend the system. For example, what if we want to add a feature to allow users to log in with their social media accounts? We would need to modify the `User` class to add properties such as `FacebookId`, `GoogleId`, etc. This would violate the Open/Closed Principle, which states that a class should be open for extension but closed for modification.

A better approach is to use composition instead of inheritance. Instead of having a `User` class that inherits from a `Person` class, we can create a `User` class that contains a `Person` object as a member variable. The `Person` class might contain properties such as `FirstName`, `LastName`, `Email`, etc. Similarly, we can create a `SocialAccount` class that contains properties such as `FacebookId`, `GoogleId`, etc.

By using composition, we can build a more flexible and modular system. For example, we can easily add new features such as social media login by creating a new `SocialAccount` object and associating it with a `User` object. We can also reuse the `Person` class in other parts of the system, such as a `Customer` class for an e-commerce application.

Here's some example code in C# that illustrates this approach:

```csharp
public class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
}

public class SocialAccount
{
    public string FacebookId { get; set; }
    public string GoogleId { get; set; }
}

public class User
{
    public int Id { get; set; }
    public DateTime CreatedAt { get; set; }
    public DateTime UpdatedAt { get; set; }
    public Person Person { get; set; }
    public SocialAccount SocialAccount { get; set; }
}
```

In this example, the `User` class contains a `Person` object and a `SocialAccount` object, which are composed together to form the user's profile. By using composition, we can build a more flexible and maintainable system that is easier to modify and extend over time.