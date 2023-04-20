# Mediator

## Simple Explanation

The Mediator design pattern is a behavioral design pattern that defines an object to encapsulate how a set of objects interact. It promotes loose coupling by preventing objects from referring to each other explicitly, allowing you to vary their interactions independently.

## Deep Explanation

The Mediator pattern is particulary useful when you have a set of objects that communicate directly with one another, leading to a complex web of dependencies. By introducing a mediator object, you can centralize communication and decouple the objects from each other, making your code modular and easier to maintain.

The main components of the Mediator pattern are:

1. Mediator - an interface or abstract class that defines methods for communication between objects (colleagues).

2. Concrete Mediator - a class implementing the Mediator interface, which encapsulates the interaction logic between colleagues.

3. Colleague - a class representing an object that interacts with other colleagues through the mediator.

## Examples

Imagine we have an application with multiple UI components that need to communicate with each other.

1. Create the Mediator interface:

```C#
public interface IMediator
{
    void Notify(object sender, string eventName);
}
```

2. Implement Concrete Mediator class:

```C#
public class ConcreteMediator : IMediator
{
    public TextBox InputBox { get; set; }
    public Button SubmitButton { get; set; }
    public Label InfoLabel { get; set; }

    public void Notify(object sender, string eventName)
    {

        if (sender == InputBox && eventName == "TextChanged") // simplified
        {
            if (!string.IsNullOrEmpty(InputBox.Text))
            {
                SubmitButton.Enabled = true;
            }
            else
            {
                SubmitButton.Enabled = false;
            }
        }
        else if (sender == SubmitButton && eventName == "Click") // simplified
        {
            InfoLabel.Text = $"Hello, {InputBox.Text}";
        }
    }
}
```

3. Create Colleague classes:

```C#
public class TextBox
{
    public IMediator Mediator { get; set; }
    public string Text { get; set; }

    public void OnTextChanged()
    {
        Mediator.Notify(this, "TextChanged");
    }
}

public class Button
{
    public IMediator Mediator { get; set; }
    public bool Enabled { get; set; }

    public void OnClick()
    {
        Mediator.Notify(this, "Click");
    }
}

public class Label
{
    public string Text { get; set; }
}
```

4. Use the Mediator and Colleague objects:

```C#
class Program
{
    static void Main(string[] args)
    {
        var mediator = new ConcreteMediator();

        var inputBox = new TextBox { Mediator = mediator };
        var submitButton = new Button { Mediator = mediator };
        var infoLabel = new Label();

        mediator.InputBox = inputBox;
        mediator.SubmitButton = submitButton;
        mediator.InfoLabel = infoLabel;

        // Simulate actions and events from UI
        inputBox.Text = "John Doe";
        inputBox.OnTextChanged();

        submitButton.OnClick();

        Console.WriteLine(infoLabel.Text); // Output: "Hello, John Doe"
    }
}
```

In this example, `TextBox`, `Button`, and `Label` are Colleague classes that communicate with each other through the `ConcreteMediator`. The `ConcreteMediator` encapsulates the interaction logic between colleagues, making it easy to change their behavior without modifying the colleague classes.

By using the Mediator pattern, you can reduce the complexity of object interactions, improve code maintainability, and promote loose coupling in your application.