# Decorator

![](../Assets/decorator.png)

## Simple Explanation

The Decorator design pattern is a structural pattern that allows you to add new functionality to an object dynamically without altering its structure. It involves wrapping an object with a decorator class that adds new behavior without modifying the original object's behavior.

## Deep Explanation

The Decorator pattern involves four main components:

1. Component - an abstract base class or interface that defines the common methods for the original object and its decorators.

2. ConcreteComponent - the actual object that needs to be extended with new functionality.

3. Decorator - an abstract class that implements the Component interface and maintains a reference to a Component object.

4. ConcreteDecorator - a class that extends the Decorator class and adds new behavior to the wrapped Component.

By using the Decorator pattern, you can attach multiple decorators to a single object, each adding new behavior without modifying the original object's code. This promotes the Single Responsibility Principle and the Open/Closed Principle by allowing you to add new functionality without changing existing code.

## Examples

Let's imagine we have a simple application for managing and displaying text with different formatting options, such as bold and italic.

1. Create the Component interface:

```C#
public interface ITextComponent
{
    string GetText();
}
```

2. Implement the ConcreteComponent class:

```C#
public class PlainText : ITextComponent
{
    private readonly string _text;

    public PlainText(string text)
    {
        _text = text;
    }

    public string GetText()
    {
        return _text;
    }
}
```

3. Implement the Decorator abstract class:

```C#
public abstract class TextDecorator : ITextComponent
{
    protected ITextComponent TextComponent;

    protected TextDecorator(ITextComponent textComponent)
    {
        TextComponent = textComponent;
    }

    public abstract string GetText();
}
```

4. Implement ConcreteDecorator classes:

```C#
public class BoldTextDecorator : TextDecorator
{
    public BoldTextDecorator(ITextComponent textComponent) : base(textComponent) { }

    public override string GetText()
    {
        return $"<b>{TextComponent.GetText()}</b>";
    }
}

public class ItalicTextDecorator : TextDecorator
{
    public ItalicTextDecorator(ITextComponent textComponent) : base(textComponent) { }

    public override string GetText()
    {
        return $"<i>{TextComponent.GetText()}</i>";
    }
}
```

5. Use the Decorator pattern:

```C#
class Program
{
    static void Main(string[] args)
    {
        ITextComponent plainText = new PlainText("Hello, world!");
        ITextComponent boldText = new BoldTextDecorator(plainText);
        ITextComponent italicBoldText = new ItalicTextDecorator(boldText);

        Console.WriteLine(plainText.GetText());
        Console.WriteLine(boldText.GetText());
        Console.WriteLine(italicBoldText.GetText());
    }
}
```

In this example, `ITextComponent` is the Component interface, `PlainText` is the ConcreteComponent class, `TextDecorator` is the Decorator abstract class, and `BoldTextDecorator` and `ItalicTextDecorator` are ConcreteDecorator classes.

The `ITextComponent` interface defines a `GetText` method for returning the formatted text. The `PlainText` class implements the `ITextComponent` interface and represents the actual text that needs to be formatted. The `TextDecorator` abstract class implements the `ITextComponent` interface and maintains a reference to a wrapped `ITextComponent` object.

The `BoldTextDecorator` and `ItalicTextDecorator` classes extend the `TextDecorator` abstract class and override the `GetText` method to add the bold and italic formatting, respectively.

This example demonstrates how the Decorator pattern can be used to extend an object's functionality dynamically without altering its structure, promoting the Single Responsibility Principle and the Open/Closed Principle.

Here's the output of the example:

```HTML
Hello, world!
<b>Hello, world!</b>
<i><b>Hello, world!</b></i>
```

As you can see, each decorator adds its specific formatting to the text. The Decorator pattern enables you to extend objects with new features in a flexible and clean way, making it easier to maintain and extend your codebase.
