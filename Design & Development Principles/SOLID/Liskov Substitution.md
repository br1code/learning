# Liskov Substitution

## Introduction

The Liskov Substitution Principle (LSP) is a design principle in software development that promotes the correct use of inheritance and polymorphism to ensure that a program should have the ability to replace any instance of a parent class with an instance of one of its child classes without affecting the correctness of the program. 

## Background

The Liskov Substitution Principle is one of the five principles of object-oriented programming and design known as SOLID, introduced by Robert C. Martin (also known as Uncle Bob) in the early 2000s. The LSP itself was formulated by Barbara Liskov in 1987, and it is an essential guideline for designing and implementing class hierarchies in an object-oriented context.

## Definition and Key Concepts

The Liskov Substitution Principle states that objects of a derived class should be able to replace objects of the base class without affecting the correctness of the program. In other words, if a class S is a subclass of class T, then an object of class T should be replaceable by an object of class S without altering the desirable properties of the program.

**From Barbara Liskov**: "A subtype should behave like a super type as far as you can tell by using the super type methods. It wasn't that it couldn't behave differently, it's just that as long as you limited your interaction with its objects to the super type methods, you would get the behavior you expect it".

To adhere to the LSP, derived classes should:
1. Honor the contracts of the base class, such as method signatures and property constraints.
2. Preserve the semantics and behavior of the base class, meaning that derived classes should not introduce unexpected side effects or violate the assumptions made about the base class.

## Benefits and Advantages

Applying the Liskov Substitution Principle in software development offers several benefits and advantages:

1. Improved code maintainability: Adhering to the LSP results in a more maintainable and robust codebase since it reduces the risk of introducing bugs when extending class hierarchies.
2. Enhanced reusability: By ensuring that derived classes can be safely substituted for their base classes, the LSP promotes the reusability of code and encourages the creation of more modular and flexible systems.
3. Easier reasoning about code: When the LSP is followed, developers can make assumptions about the behavior of derived classes based on their knowledge of the base class, simplifying the process of understanding and reasoning about the code.
4. Increased robustness: By preserving the contracts and semantics of base classes, the LSP ensures that systems remain stable and predictable even as class hierarchies evolve and expand.

## Challenges and Limitations

1. Design complexity: Adhering to the Liskov Substitution Principle may require additional effort in designing class hierarchies, as developers need to ensure that derived classes adhere to the contracts and semantics of their base classes.
2. Inheritance misuse: If not applied properly, the LSP can lead to inappropriate use of inheritance, resulting in complex and hard-to-maintain class hierarchies.
3. Performance concerns: Ensuring compliance with the LSP may sometimes require additional levels of abstraction, which can introduce performance overhead in certain scenarios.

## Best Practices and Guidelines

1. Favor composition over inheritance: Use composition to build complex behaviors by combining simpler, more focused components, which can help avoid violating the LSP.
2. Be cautious with method overriding: When overriding methods in derived classes, ensure that the new implementation preserves the semantics and contracts of the base class.
3. Use interface segregation: Break down large interfaces into smaller, more focused interfaces, making it easier for derived classes to comply with the LSP.
4. Leverage the "is-a" relationship: Ensure that inheritance is used only when the derived class truly "is-a" specialization of the base class, which helps maintain LSP compliance.

Identifying when the Liskov Substitution Principle (LSP) is being violated can be challenging, but there are some common indicators to watch out for. In general, the LSP is violated when a subclass does not behave as expected when used in place of its parent class. Some specific signs of LSP violations include:

1. **Unexpected exceptions:** When a subclass throws an exception for a method that the parent class expects to be used without issue, it may indicate an LSP violation. This can lead to unexpected behavior when the subclass is substituted for the parent class.

2. **Altered behavior:** If a subclass changes the behavior of a parent class's method in a way that contradicts the parent class's intended behavior, it may violate the LSP. For example, if a parent class's method is expected to return a positive value, but the subclass's overridden method returns a negative value, this could lead to incorrect results when using the subclass in place of the parent class.

3. **Preconditions strengthening:** If a subclass imposes stronger preconditions on its methods than the parent class, it may violate the LSP. This means that the subclass has stricter requirements for the input parameters of a method, which could cause issues when the subclass is used as a substitute for the parent class.

4. **Postconditions weakening:** Conversely, if a subclass provides weaker postconditions (i.e., the expected outcomes) than the parent class, it could also violate the LSP. This may lead to unexpected results when the subclass is used as a substitute for the parent class.

5. **Invariants violation:** If a subclass does not maintain the same invariants as the parent class, it may violate the LSP. Invariants are conditions that should always be true for an object during its lifetime. If a subclass breaks these conditions, it can lead to unexpected behavior.

To identify LSP violations, you can review your code and look for these indicators. Be particularly attentive to situations where you feel the need to check the type of an object before invoking a method, as this can be a sign that the LSP is being violated. If you find potential violations, consider refactoring the code to better adhere to the LSP, such as by introducing abstractions or modifying the class hierarchy to better model the problem domain.

## Example

Consider a simple example of a `Bird` class with a method `Fly`:

```csharp
public class Bird
{
    public virtual void Fly()
    {
        // Code for flying
    }
}

public class Pigeon : Bird
{
    public override void Fly()
    {
        // Code for pigeon flying
    }
}

public class Penguin : Bird
{
    public override void Fly()
    {
        throw new NotSupportedException("Penguins can't fly.");
    }
}
```

In this example, the `Penguin` class violates the Liskov Substitution Principle because it introduces an exception in the `Fly` method, which breaks the contract of the base `Bird` class. To adhere to the LSP, we can refactor the code using interfaces:

```csharp
public interface IFlyable
{
    void Fly();
}

public class Bird
{
    // Common bird properties and methods
}

public class Pigeon : Bird, IFlyable
{
    public void Fly()
    {
        // Code for pigeon flying
    }
}

public class Penguin : Bird
{
    // Penguin-specific properties and methods
}
```

Now, the `Penguin` class no longer violates the LSP, as it doesn't inherit the `Fly` method from the base `Bird` class, and the `Pigeon` class implements the `IFlyable` interface to provide the flying behavior.

---

Let's consider a real-world example using a user authentication system. In this system, we have a `User` class with a `ChangePassword` method. We also have an `AdminUser` class, which is a subclass of `User` and has additional privileges. Suppose our initial implementation looks like this:

```csharp
public class User
{
    public string Password { get; private set; }

    public virtual void ChangePassword(string oldPassword, string newPassword)
    {
        if (Password == oldPassword)
        {
            Password = newPassword;
        }
        else
        {
            throw new InvalidOperationException("Old password does not match.");
        }
    }
}

public class AdminUser : User
{
    public override void ChangePassword(string oldPassword, string newPassword)
    {
        if (newPassword.Length < 10)
        {
            throw new ArgumentException("New password must be at least 10 characters long.");
        }
        base.ChangePassword(oldPassword, newPassword);
    }
}
```

In this example, the `AdminUser` class inherits from the `User` class and overrides the `ChangePassword` method. However, this implementation violates the Liskov Substitution Principle because it introduces a new precondition that the `newPassword` parameter must be at least 10 characters long. This makes it impossible to use the `AdminUser` class as a substitute for the `User` class without altering the expected behavior.

To fix the violation of the LSP, we can refactor the classes and introduce a protected method that handles password validation. This way, we can extend the validation rules in the subclass without violating the LSP:

```csharp
public class User
{
    public string Password { get; private set; }

    public void ChangePassword(string oldPassword, string newPassword)
    {
        if (Password == oldPassword)
        {
            ValidateNewPassword(newPassword);
            Password = newPassword;
        }
        else
        {
            throw new InvalidOperationException("Old password does not match.");
        }
    }

    protected virtual void ValidateNewPassword(string newPassword)
    {
        // Default validation for regular users (can be empty if no specific rules apply)
    }
}

public class AdminUser : User
{
    protected override void ValidateNewPassword(string newPassword)
    {
        if (newPassword.Length < 10)
        {
            throw new ArgumentException("New password must be at least 10 characters long.");
        }
    }
}
```

In this refactored version, both `User` and `AdminUser` classes use the same `ChangePassword` method, and the new password validation rules are encapsulated in a separate `ValidateNewPassword` method. The `AdminUser` class extends the validation rules by overriding this method, while still adhering to the Liskov Substitution Principle.

---

Let's consider another real-world example using a simple invoicing system. In this system, we have a `RegularInvoice` class, which calculates the total invoice amount by adding taxes. We also have a `TaxExemptInvoice` class, which is a special case where taxes should not be added. Suppose our initial implementation looks like this:

```csharp
public class RegularInvoice
{
    public decimal Subtotal { get; set; }
    public decimal TaxRate { get; set; }

    public virtual decimal CalculateTotal()
    {
        return Subtotal + (Subtotal * TaxRate);
    }
}

public class TaxExemptInvoice : RegularInvoice
{
    public override decimal CalculateTotal()
    {
        throw new NotSupportedException("TaxExemptInvoice does not support tax calculations.");
    }
}
```

In this example, the `TaxExemptInvoice` class inherits from the `RegularInvoice` class and overrides the `CalculateTotal` method. However, this implementation violates the Liskov Substitution Principle because it changes the behavior of the `CalculateTotal` method by throwing a `NotSupportedException`, making it impossible to use the `TaxExemptInvoice` class as a substitute for the `RegularInvoice` class without altering the expected behavior.

To fix the violation of the LSP, we can introduce an abstraction, such as an interface or an abstract base class, and refactor the classes to conform to this abstraction:

```csharp
public interface IInvoice
{
    decimal Subtotal { get; set; }
    decimal CalculateTotal();
}

public class RegularInvoice : IInvoice
{
    public decimal Subtotal { get; set; }
    public decimal TaxRate { get; set; }

    public decimal CalculateTotal()
    {
        return Subtotal + (Subtotal * TaxRate);
    }
}

public class TaxExemptInvoice : IInvoice
{
    public decimal Subtotal { get; set; }

    public decimal CalculateTotal()
    {
        return Subtotal;
    }
}
```

In this refactored version, both `RegularInvoice` and `TaxExemptInvoice` implement the `IInvoice` interface, allowing them to be used interchangeably without violating the Liskov Substitution Principle. The `TaxExemptInvoice` class no longer throws an exception in the `CalculateTotal` method and instead returns the correct value for tax-exempt invoices.

## Conclusion

The Liskov Substitution Principle is a crucial design principle in software development that promotes the proper use of inheritance and polymorphism to ensure that derived classes can be safely substituted for their base classes without affecting the program's correctness. By adhering to the LSP, developers can create more maintainable, reusable, and robust software systems. While there are challenges and limitations, such as design complexity and potential performance concerns, the benefits of applying the LSP generally outweigh these drawbacks, leading to more stable and flexible class hierarchies in object-oriented programming.