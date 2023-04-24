# OOP

## Introduction

### Definition and overview of Object-Oriented Programming

Object-Oriented Programming (OOP) is a programming paradigm that uses the concept of "objects" to model real-world entities and represent them in software. Objects are instances of classes, which define the structure and behavior of these entities. OOP aims to make software development more modular, maintainable, and scalable by organizing code around objects, promoting reusability, and enforcing separation of concerns.

In OOP, objects encapsulate data (attributes) and operations (methods) that can be performed on that data. This encapsulation provides a layer of abstraction, allowing developers to work with objects without needing to understand their internal implementation details. Objects can interact with one another through well-defined interfaces, facilitating communication and collaboration between different parts of a software system.

The main principles of OOP are:

1. Abstraction: Simplifying complex entities by breaking them down into smaller, more manageable parts, and providing a simplified interface to work with those parts.
2. Encapsulation: Hiding the internal state and implementation details of an object and exposing a well-defined interface for interaction.
3. Inheritance: Creating new classes by reusing and extending the properties and behavior of existing classes, promoting code reusability and modularity.
4. Polymorphism: Allowing objects of different classes to be treated as objects of a common superclass, providing a unified interface for interacting with related objects.

OOP languages, such as Java, C++, C#, and Python, provide features and constructs that support the implementation of these principles, including classes, objects, methods, and inheritance. By adhering to the principles of OOP, developers can create software systems that are easier to understand, modify, and maintain, ultimately improving the overall quality and longevity of the software.

### Comparison with procedural and functional programming paradigms

1. Object-Oriented Programming (OOP):
   - Focuses on organizing code around objects, which are instances of classes representing real-world entities.
   - Encourages encapsulation, abstraction, inheritance, and polymorphism.
   - Aims to promote code reusability, modularity, and maintainability.
   - Examples of OOP languages: Java, C++, C#, Python

2. Procedural Programming:
   - Focuses on organizing code around procedures (also known as functions or routines) that perform specific tasks.
   - Relies on a top-down approach, breaking a problem into a series of smaller tasks or subproblems, which are solved by individual procedures.
   - Data and procedures are separate entities. Procedures manipulate data passed to them as arguments.
   - Can lead to less modular and maintainable code when compared to OOP, as procedures can become interdependent and data can be more exposed.
   - Examples of procedural languages: C, Pascal, Fortran

3. Functional Programming:
   - Focuses on organizing code around mathematical functions and immutable data.
   - Encourages the use of pure functions, which have no side effects and always produce the same output given the same input.
   - Promotes the use of higher-order functions, which can take other functions as arguments or return them as results.
   - Emphasizes immutability, minimizing the use of shared state and mutable data, which can lead to fewer bugs and better concurrency support.
   - Can result in more concise and expressive code, but may have a steeper learning curve for developers unfamiliar with the functional paradigm.
   - Examples of functional languages: Haskell, Lisp, Erlang, F#, Scala

In summary, the main differences between the paradigms are:

- OOP organizes code around objects and focuses on encapsulation, abstraction, inheritance, and polymorphism to promote modularity and maintainability.
- Procedural programming organizes code around procedures that perform specific tasks and follows a top-down approach, which can lead to less modular and maintainable code when compared to OOP.
- Functional programming organizes code around mathematical functions, emphasizing immutability, purity, and higher-order functions, resulting in more concise and expressive code with fewer side effects and better concurrency support.

Each paradigm has its strengths and weaknesses, and the choice of which paradigm to use depends on factors such as the problem domain, project requirements, and the preferences and expertise of the development team. It's also worth noting that many modern programming languages support multiple paradigms, allowing developers to use a combination of OOP, procedural, and functional approaches as needed.

## Core Concepts

### Objects

An object is an instance of a class that represents a real-world entity or concept in object-oriented programming. Objects have a state and behavior. The state is represented by attributes (also known as properties or fields), and the behavior is represented by methods (also known as functions or operations).

#### Properties

Properties are variables that store the data or state of an object. They are typically defined within a class and can have different access levels (public, private, or protected) to control their visibility and usage.

#### Methods

Methods are functions that define the behavior of an object. They operate on the object's properties and can be used to perform actions, manipulate the object's state, or retrieve information about the object.

### Classes

A class is a blueprint or template for creating objects in object-oriented programming. It defines the structure, properties, and methods that will be shared by all instances of the class. Classes are used to encapsulate the data and behavior of a real-world entity or concept and provide a reusable and modular structure for creating objects.

#### Constructors

Constructors are special methods in a class that are used to initialize a new object. They are called when an object is created and can take arguments to set the initial state of the object. Constructors can have different access levels (public, private, or protected) and can be overloaded, meaning a class can have multiple constructors with different parameter sets.

#### Class Hierarchies

Class hierarchies refer to the relationships between classes in object-oriented programming, particularly through inheritance. Inheritance allows a class to inherit the properties and methods of another class, forming a parent-child relationship between the two classes. The parent class is referred to as the superclass or base class, and the child class is referred to as the subclass or derived class. Class hierarchies enable code reuse, modularity, and the implementation of polymorphism.

### Abstraction

Abstraction is the process of simplifying complex systems by breaking them down into smaller, more manageable parts, and providing a simplified interface for interacting with those parts. In object-oriented programming, abstraction helps developers to manage complexity, promote code reusability, and enforce separation of concerns by hiding the internal details of objects and exposing only the essential features that are relevant to the users of those objects.

In OOP, abstraction can be achieved through:

1. Abstract classes: Abstract classes are classes that cannot be instantiated and act as a base for other classes. They can contain both abstract and concrete methods. Abstract methods have no implementation in the abstract class and must be implemented by any derived class. Abstract classes allow you to define common behavior and structure for related classes, without providing a complete implementation.

2. Interfaces: Interfaces define a contract specifying a set of methods that a class must implement. They provide a way to define reusable behavior without specifying the implementation, allowing multiple classes to implement the same interface and adhere to the same contract. This enables a higher level of abstraction by allowing objects of different classes to be treated as objects of a common interface.

Here's a C# example demonstrating abstraction using both abstract classes and interfaces:

```csharp
// Define an interface for a shape
public interface IShape
{
    double GetArea();
    double GetPerimeter();
}

// Define an abstract base class for shapes with common properties and methods
public abstract class Shape : IShape
{
    public string Color { get; set; }

    public abstract double GetArea();
    public abstract double GetPerimeter();
}

// Define a Circle class that inherits from the abstract Shape class and implements the IShape interface
public class Circle : Shape
{
    public double Radius { get; set; }

    public Circle(double radius, string color)
    {
        Radius = radius;
        Color = color;
    }

    public override double GetArea()
    {
        return Math.PI * Math.Pow(Radius, 2);
    }

    public override double GetPerimeter()
    {
        return 2 * Math.PI * Radius;
    }
}

// Define a Rectangle class that inherits from the abstract Shape class and implements the IShape interface
public class Rectangle : Shape
{
    public double Width { get; set; }
    public double Height { get; set; }

    public Rectangle(double width, double height, string color)
    {
        Width = width;
        Height = height;
        Color = color;
    }

    public override double GetArea()
    {
        return Width * Height;
    }

    public override double GetPerimeter()
    {
        return 2 * (Width + Height);
    }
}
```

In this example, the `IShape` interface defines a contract for shapes, the abstract `Shape` class provides common properties and methods, and the `Circle` and `Rectangle` classes provide specific implementations for their respective shapes. The use of abstraction in this example allows you to work with different shape objects through a common interface, while hiding the specific implementation details of each shape.

### Encapsulation

Encapsulation is the principle of bundling data (attributes) and operations (methods) that act on that data within a single unit, typically an object. Encapsulation is a mechanism for hiding the internal state and implementation details of an object, while exposing a well-defined interface for interaction. This promotes modularity, maintainability, and separation of concerns by allowing you to change the implementation details of an object without affecting the users of that object.

In OOP, encapsulation can be achieved through:

1. Access Modifiers: Controlling the visibility of class members (properties, methods, and fields) using access modifiers like public, private, and protected. This helps you to restrict access to the internal details of an object and enforce a clean interface for interaction.

2. Properties: Using properties to encapsulate fields, allowing you to control how the fields are accessed and modified. Properties can have get and set accessors, enabling you to apply validation or perform other actions when the values of fields are accessed or changed.

Here's a C# example demonstrating encapsulation:

```csharp
public class BankAccount
{
    // Private field to store the balance
    private decimal _balance;

    // Public property to access and modify the balance with validation
    public decimal Balance
    {
        get
        {
            return _balance;
        }
        set
        {
            if (value >= 0)
            {
                _balance = value;
            }
            else
            {
                throw new ArgumentException("Balance cannot be negative.");
            }
        }
    }

    // Public method to deposit money into the account
    public void Deposit(decimal amount)
    {
        if (amount > 0)
        {
            Balance += amount;
        }
        else
        {
            throw new ArgumentException("Deposit amount must be positive.");
        }
    }

    // Public method to withdraw money from the account
    public void Withdraw(decimal amount)
    {
        if (amount > 0 && amount <= Balance)
        {
            Balance -= amount;
        }
        else
        {
            throw new ArgumentException("Invalid withdrawal amount.");
        }
    }

    // Private method to calculate interest (not accessible from outside the class)
    private decimal CalculateInterest()
    {
        return Balance * 0.05m;
    }
}
```

In this example, the `BankAccount` class encapsulates the `_balance` field by providing a public property `Balance` with validation logic. It also provides public methods `Deposit` and `Withdraw` to modify the balance, and a private method `CalculateInterest` which is not accessible from outside the class. Encapsulation helps to ensure that the internal state of the `BankAccount` object is maintained in a consistent and valid state, while hiding the implementation details and providing a clean interface for interaction.

### Inheritance

Inheritance in OOP:

Inheritance is a mechanism in object-oriented programming that allows a class to inherit the properties and methods of another class, promoting code reusability and modularity. Inheritance forms a parent-child relationship between two classes, where the parent class is referred to as the superclass or base class, and the child class is referred to as the subclass or derived class. The subclass inherits the characteristics of the superclass, and can also add or override its own properties and methods.

Inheritance enables you to create new classes based on existing ones, reducing duplication and promoting the reuse of code. It also helps in modeling real-world relationships between entities, representing "is-a" relationships, where the subclass is a more specialized version of the superclass.

Here's a C# example demonstrating inheritance:

Let's consider a real-world example involving a company's employee management system. We'll define a base `Employee` class and then create subclasses for different employee types, such as `Manager` and `Developer`.

```csharp
// Define a base Employee class
public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Salary { get; set; }

    public Employee(int id, string name, decimal salary)
    {
        Id = id;
        Name = name;
        Salary = salary;
    }

    public virtual void Work()
    {
        Console.WriteLine($"{Name} is working.");
    }
}

// Define a Manager class that inherits from the Employee class
public class Manager : Employee
{
    public Manager(int id, string name, decimal salary) : base(id, name, salary)
    {
    }

    public override void Work()
    {
        Console.WriteLine($"{Name} is managing the team.");
    }

    public void EvaluatePerformance()
    {
        Console.WriteLine($"{Name} is evaluating the team's performance.");
    }
}

// Define a Developer class that inherits from the Employee class
public class Developer : Employee
{
    public string ProgrammingLanguage { get; set; }

    public Developer(int id, string name, decimal salary, string programmingLanguage)
        : base(id, name, salary)
    {
        ProgrammingLanguage = programmingLanguage;
    }

    public override void Work()
    {
        Console.WriteLine($"{Name} is coding in {ProgrammingLanguage}.");
    }

    public void DebugCode()
    {
        Console.WriteLine($"{Name} is debugging code.");
    }
}

// Usage
Manager manager = new Manager(1, "Alice", 90000m);
manager.Work(); // Output: "Alice is managing the team."
manager.EvaluatePerformance(); // Output: "Alice is evaluating the team's performance."

Developer developer = new Developer(2, "Bob", 80000m, "C#");
developer.Work(); // Output: "Bob is coding in C#."
developer.DebugCode(); // Output: "Bob is debugging code."
```

In this example, the `Manager` and `Developer` classes inherit from the `Employee` base class, reusing the properties `Id`, `Name`, and `Salary`, as well as the constructor. Each subclass provides its own implementation of the `Work` method, overriding the base class method, and adds additional methods specific to their roles (`EvaluatePerformance` for `Manager` and `DebugCode` for `Developer`). This inheritance structure models the real-world relationships between employees and their roles, promoting code reuse and modularity.

### Polymorphism

Polymorphism is a principle in object-oriented programming that allows objects of different classes to be treated as objects of a common superclass or interface. It enables a single interface or method to work with different types, providing flexibility and extensibility in your code. Polymorphism promotes code reusability and separation of concerns by allowing you to write generic code that works with a range of classes, without needing to know the specific class type at compile time.

There are two main types of polymorphism in OOP:

Compile-time polymorphism (also known as static or early binding): This type of polymorphism is achieved through method overloading, where multiple methods have the same name but different parameters. The correct method to call is determined at compile time based on the method signature.

Runtime polymorphism (also known as dynamic or late binding): This type of polymorphism is achieved through method overriding and interfaces, where a subclass or implementing class provides its own implementation of a method. The correct method to call is determined at runtime based on the actual object type.

Here's a real-world C# example demonstrating polymorphism:

Let's consider a real-world example involving a company's employee management system. We'll use polymorphism to handle employee tasks in a generic way.

```csharp
// Define an abstract base class for employees with a common method
public abstract class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }

    public Employee(int id, string name)
    {
        Id = id;
        Name = name;
    }

    public abstract void PerformTask();
}

// Define a Manager class that inherits from the Employee class
public class Manager : Employee
{
    public Manager(int id, string name) : base(id, name)
    {
    }

    public override void PerformTask()
    {
        Console.WriteLine($"{Name} is managing the team.");
    }
}

// Define a Developer class that inherits from the Employee class
public class Developer : Employee
{
    public string ProgrammingLanguage { get; set; }

    public Developer(int id, string name, string programmingLanguage) : base(id, name)
    {
        ProgrammingLanguage = programmingLanguage;
    }

    public override void PerformTask()
    {
        Console.WriteLine($"{Name} is coding in {ProgrammingLanguage}.");
    }
}

// Usage
List<Employee> employees = new List<Employee>
{
    new Manager(1, "Alice"),
    new Developer(2, "Bob", "C#")
};

foreach (Employee employee in employees)
{
    employee.PerformTask();
}
```

In this example, we have an abstract `Employee` class with an abstract `PerformTask` method, and two subclasses `Manager` and `Developer` that provide their own implementations of `PerformTask`. Polymorphism allows us to treat objects of the `Manager` and `Developer` classes as objects of the common `Employee` superclass, and call the `PerformTask` method on them without knowing their specific type at compile time. The correct implementation of the `PerformTask` method is called at runtime based on the actual object type, demonstrating runtime polymorphism.

## Related Design Principles

TODO: create links to existing documents

- SOLID Principles
  - Single Responsibility Principle (SRP)
  - Open/Closed Principle (OCP)
  - Liskov Substitution Principle (LSP)
  - Interface Segregation Principle (ISP)
  - Dependency Inversion Principle (DIP)
- Composition over Inheritance
- Separation of Concerns
- Law of Demeter
- DRY (Don't Repeat Yourself) Principle
- KISS (Keep It Simple, Stupid) Principle

## Related Design Patterns

TODO: create links to existing documents

- Creational Patterns
  - Singleton
  - Factory Method
  - Abstract Factory
  - Builder
  - Prototype
- Structural Patterns
  - Adapter
  - Bridge
  - Composite
  - Decorator
  - Facade
  - Flyweight
  - Proxy
- Behavioral Patterns
  - Chain of Responsibility
  - Command
  - Interpreter
  - Iterator
  - Mediator
  - Memento
  - Observer
  - State
  - Strategy
  - Template Method
  - Visitor

## OOP In Practice

### Practical examples and case studies

### Tips for writing clean, maintainable, and scalable OOP code

### Testing and debugging OOP applications

### Code refactoring techniques and tips