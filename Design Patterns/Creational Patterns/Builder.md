# Builder

![](../Assets/builder.png)

## Simple Explanation

The Builder design pattern is a creational pattern that separates the construction of a complex object from its representation. It allows for the step-by-step creation of complex objects using a specific sequence of actions, resulting in a more readable and maintainable code.

## Deep Explanation

The Builder pattern involves four main components:

1. Product - the complex object being built.

2. Builder - an abstract class or interface that specifies the methods for constructing the Product.

3. ConcreteBuilder - a class that implements the Builder interface, providing specific implementations for constructing the Product.

4. Director - a class that takes a Builder instance and uses its methods to construct a Product. The Director knows the construction sequence but doesn't know the details of the construction.

The Builder pattern is useful when you need to create complex objects with multiple steps or when you need to create different representations of an object. It helps to keep the construction logic separate from the representation of the object, making it easier to maintain and extend the code.

## Examples

Let's imagine we have a simple application that creates and processes sandwiches with various ingredients.

1. Create the Product class:

```C#
public class Sandwich
{
    public List<string> Ingredients { get; } = new List<string>();

    public void DisplayIngredients()
    {
        Console.WriteLine(string.Join(", ", Ingredients));
    }
}
```

2. Implement the Builder interface:

```C#
public interface ISandwichBuilder
{
    void AddBread();
    void AddMeat();
    void AddCheese();
    void AddVegetables();
    void AddCondiments();
    Sandwich GetSandwich();
}
```

3. Implement the ConcreteBuilder class:

```C#
public class TurkeyClubBuilder : ISandwichBuilder
{
    private Sandwich _sandwich;

    public TurkeyClubBuilder()
    {
        _sandwich = new Sandwich();
    }

    public void AddBread() => _sandwich.Ingredients.Add("Wheat Bread");
    public void AddMeat() => _sandwich.Ingredients.Add("Turkey");
    public void AddCheese() => _sandwich.Ingredients.Add("Swiss Cheese");
    public void AddVegetables() => _sandwich.Ingredients.Add("Lettuce and Tomato");
    public void AddCondiments() => _sandwich.Ingredients.Add("Mayonnaise");

    public Sandwich GetSandwich()
    {
        return _sandwich;
    }
}
```

4. Implement the Director class:

```C#
public class SandwichMaker
{
    private readonly ISandwichBuilder _builder;

    public SandwichMaker(ISandwichBuilder builder)
    {
        _builder = builder;
    }

    public void BuildSandwich()
    {
        _builder.AddBread();
        _builder.AddMeat();
        _builder.AddCheese();
        _builder.AddVegetables();
        _builder.AddCondiments();
    }

    public Sandwich GetSandwich()
    {
        return _builder.GetSandwich();
    }
}
```

5. Use the Builder pattern:

```C#
class Program
{
    static void Main(string[] args)
    {
        ISandwichBuilder builder = new TurkeyClubBuilder();
        SandwichMaker sandwichMaker = new SandwichMaker(builder);

        sandwichMaker.BuildSandwich();
        Sandwich sandwich = sandwichMaker.GetSandwich();

        sandwich.DisplayIngredients();
    }
}
```

In this example, `Sandwich` is the Product class, `ISandwichBuilder` is the Builder interface, `TurkeyClubBuilder` is the ConcreteBuilder class, and `SandwichMaker` is the Director class. The `Sandwich` class has a list of ingredients and a method to display the ingredients. The `ISandwichBuilder` interface defines the methods required to construct a `Sandwich` object step-by-step.

The `TurkeyClubBuilder` class implements the `ISandwichBuilder` interface, providing specific implementations for adding bread, meat, cheese, vegetables, and condiments to the `Sandwich`. The `GetSandwich` method returns the constructed `Sandwich`.

The `SandwichMaker` class, which is the Director, takes an `ISandwichBuilder` instance and uses its methods to construct a `Sandwich`. The `BuildSandwich` method defines the sequence of steps required to create the sandwich. The `GetSandwich` method returns the fully constructed Sandwich.

In the `Main` method of the `Program` class, we create an instance of the `TurkeyClubBuilder`, pass it to the `SandwichMaker`, and then call `BuildSandwich` to create the sandwich. After that, we retrieve the constructed `Sandwich` using the `GetSandwich` method and display its ingredients.

This example demonstrates how the Builder pattern can be used to create complex objects step-by-step, keeping the construction logic separate from the object's representation. By using different ConcreteBuilder classes, you can create different representations of the `Sandwich` object, making the construction process more flexible and maintainable.