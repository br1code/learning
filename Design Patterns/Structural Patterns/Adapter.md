# Adapter

![](../Assets/adapter.png)

## Simple Explanation

The Adapter design pattern is a structural pattern that allows two incompatible interfaces to work together by converting the interface of one class into another interface that the client expects. It helps to integrate existing components or libraries into your application without changing their source code.

## Deep Explanation

The Adapter pattern involves three main components:

1. Target - the interface that the client expects and understands.

2. Adaptee - the existing class or interface that is incompatible with the Target interface.

3. Adapter - a class that implements the Target interface and translates calls from the Target interface to the Adaptee, allowing them to work together.

The Adapter pattern is useful when you need to use a class or library that has an interface incompatible with your application. Instead of modifying the existing class or library, you can create an adapter that translates between the expected interface and the existing interface, allowing them to work together.

## Examples

Let's imagine we have a simple application that displays messages in different formats, and we want to integrate a third-party library that displays messages in a format that is incompatible with our application.

1. Create the Target interface:

```C#
public interface IMessageDisplayer
{
    void DisplayMessage(string message);
}
```

2. Implement the Adaptee class:

```C#
public class ThirdPartyMessageDisplayer
{
    public void Show(string text)
    {
        Console.WriteLine($"[ThirdParty] {text}");
    }
}
```

3. Implement the Adapter class:

```C#
public class MessageDisplayerAdapter : IMessageDisplayer
{
    private readonly ThirdPartyMessageDisplayer _adaptee;

    public MessageDisplayerAdapter(ThirdPartyMessageDisplayer adaptee)
    {
        _adaptee = adaptee;
    }

    public void DisplayMessage(string message)
    {
        _adaptee.Show(message);
    }
}
```

4. Use the Adapter pattern:

```C#
class Program
{
    static void Main(string[] args)
    {
        IMessageDisplayer displayer = new MessageDisplayerAdapter(new ThirdPartyMessageDisplayer());
        displayer.DisplayMessage("Hello, Adapter Pattern!");
    }
}
```

In this example, `IMessageDisplayer` is the Target interface, `ThirdPartyMessageDisplayer` is the Adaptee class, and `MessageDisplayerAdapter` is the Adapter class. The `IMessageDisplayer` interface has a single method `DisplayMessage` that the client expects and understands.

The `ThirdPartyMessageDisplayer` class represents a third-party library that has a different method, `Show`, for displaying messages. To make it compatible with our application, we create the `MessageDisplayerAdapter` class that implements the `IMessageDisplayer` interface and translates calls from the `DisplayMessage` method to the `Show` method of the `ThirdPartyMessageDisplayer`.

This example demonstrates how the Adapter pattern can be used to integrate incompatible interfaces and allow them to work together without modifying their source code. By using an adapter, you can seamlessly integrate third-party libraries or existing components into your application, improving its flexibility and maintainability.