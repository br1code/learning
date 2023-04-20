# Command

![](../Assets/command.png)

## Simple Explanation

The Command design pattern is a behavioral design pattern that encapsulates a request or operation as an object, allowing you to decouple the object that invokes the operation from the object that performs it. This provides greater flexibility in managing operations, such as supporting undo/redo functionality, implementing callbacks, or queuing and executing requests at different times.

## Deep Explanation

The Command pattern consists of three main components:

1. Command - an interface or abstract class that defines the common methods for executing a command.

2. Concrete Command - a class implementing the Command interface, which contains the specific logic for executing the command.

3. Invoker - an object that knows how to execute a command but doesn't know anything about the specifics of the command.

This pattern allows you to centralize and manage the execution logic, making it easy to extend or modify the behavior without changing the classes that use the command objects. Additionally, it promotes the Single Responsibility Principle and Open/Closed Principle, making your code more modular and maintainable.

## Examples

Imagine we have an application that controls a smart home system with lights and air conditioners. We can create command objects to manage these devices.

1. Create the Command interface:

```C#
public interface ICommand
{
    void Execute();
    void Undo();
}
```

2. Implement Concrete Command Classes:

```C#
public class TurnOnLightCommand : ICommand
{
    private readonly Light _light;

    public TurnOnLightCommand(Light light)
    {
        _light = light;
    }

    public void Execute()
    {
        _light.TurnOn();
    }

    public void Undo()
    {
        _light.TurnOff();
    }
}

public class TurnOnAirConditionerCommand : ICommand
{
    private readonly AirConditioner _airConditioner;

    public TurnOnAirConditionerCommand(AirConditioner airConditioner)
    {
        _airConditioner = airConditioner;
    }

    public void Execute()
    {
        _airConditioner.TurnOn();
    }

    public void Undo()
    {
        _airConditioner.TurnOff();
    }
}
```

3. Create the Invoker class:

```C#
public class SmartHomeController
{
    private ICommand _command;

    public void SetCommand(ICommand command)
    {
        _command = command;
    }

    public void ExecuteCommand()
    {
        _command.Execute();
    }

    public void UndoCommand()
    {
        _command.Undo();
    }
}
```

4. Use the Command objects with the Invoker:

```C#
class Program
{
    static void Main(string[] args)
    {
        Light livingRoomLight = new Light();
        AirConditioner bedroomAirConditioner = new AirConditioner();

        ICommand turnOnLightCommand = new TurnOnLightCommand(livingRoomLight);
        ICommand turnOnAirConditionerCommand = new TurnOnAirConditionerCommand(bedroomAirConditioner);

        SmartHomeController controller = new SmartHomeController();

        // Turn on the light
        controller.SetCommand(turnOnLightCommand);
        controller.ExecuteCommand();

        // Turn on the air conditioner
        controller.SetCommand(turnOnAirConditionerCommand);
        controller.ExecuteCommand();

        // Undo the last command (turn off the air conditioner)
        controller.UndoCommand();
    }
}
```

In this example, `TurnOnLightCommand` and `TurnOnAirConditionerCommand` are concrete commands implementing the `ICommand` interface. The `SmartHomeController` acts as the invoker, which manages and executes commands. This allows you to easily add new commands or modify existing ones without changing the invoker or other parts of the system that use the commands. The Command pattern provides a flexible and maintainable way to manage operations, making it easier to extend and modify the behavior of your application as requirements change.