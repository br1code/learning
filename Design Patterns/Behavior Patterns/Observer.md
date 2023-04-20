# Observer

## Simple Explanation

The observer design pattern is a behavioral design pattern that defines a one-to-many dependency between objects so that when one object (the subject) changes its state, all its dependents (observers) are notified and updated automatically. It promotes loose coupling between the subject and its observers, making your code more modular and easier to maintain.

## Deep Explanation

The Observer pattern consists of two main components:

1. Subject - an interface or abstract class that defines methods for registering, unregistering, and notifying observers.

2. Observer - an interface or abstract class that defines a method for receiving updates from the subject.

By implementing the Observer pattern, you can allow multiple observers to react to state changes in the subject wihout hardcoding the relationships betwen them. This makes it easy to add, remove, or modify observers without affecting the subject or others observers.

## Examples

Imagine we have a weather station that measures temperature, and we want to display the temperature on multiple devices.

1. Create the Subject interface:

```C#
public interface ISubject
{
    void RegisterObserver(IObserver observer);
    void RemoveObserver(IObserver observer);
    void NotifyObservers();
}
```

2. Create the Observer interface:

```C#
public interface IObserver
{
    void Update(float temperature);
}
```

3. Implement the Concrete Subject class:

```C#
public class WeatherStation : ISubject
{
    private readonly List<IObserver> _observers = new List<IObserver>();
    private float _temperature;

    public void RegisterObserver(IObserver observer)
    {
        _observers.Add(observer);
    }

    public void RemoveObserver(IObserver observer)
    {
        _observers.Remove(observer);
    }

    public void NotifyObservers()
    {
        foreach (IObserver observer in _observers)
        {
            observer.Update(_temperature);
        }
    }

    public void SetTemperature(float temperature)
    {
        _temperature = temperature;
        NotifyObservers();
    }
}
```

4. Implement Concrete Observer classes:

```C#
public class PhoneDisplay : IObserver
{
    public void Update(float temperature)
    {
        Console.WriteLine($"Phone Display: The temperature is {temperature}");
    }
}

public class TabletDisplay : IObserver
{
    public void Update(float temperature)
    {
        Console.WriteLine($"Tablet Display: The temperature is {temperature}");
    }
}
```

5. Use the Subject and Observer objects:

```C#
class Program
{
    static void Main(string[] args)
    {
        // Subject
        var weatherStation = new WeatherStation();
        
        // Observers
        var phoneDisplay = new PhoneDisplay();
        var tabletDisplay = new TabletDisplay();

        weatherStation.RegisterObserver(phoneDisplay);
        weatherStation.RegisterObserver(tabletDisplay);

        weatherStation.SetTemperature(25.0f);

        /* Output:
            Phone Display: The temperature is 25.0
            Tablet Display: The temperature is 25.0
        */
    }
}
```

In this example, `WeatherStation` is the Concrete Subject, and `PhoneDisplay` and `TabletDisplay` are Concrete Observers. When the temperature changes in the `WeatherStation`, it notifies the registered observers, which then update their displays accordingly.

By using the Observer pattern, you can create a flexible and modular system that allows multiple observers to react to changes in the subject without having to modify the subject or other observers. This promotes loose coupling and improves code maintainability.