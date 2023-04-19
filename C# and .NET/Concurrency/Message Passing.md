# Message Passing

TODO: Watch https://youtu.be/E0Ld7ZgE4oY

## Simple Explanation

Message Passing is a technique of inter-process communication where **different processes or threads can communicate with each other by sending messages through a channel**. In C# and .NET, Message Passing can be implemented using various constructs such as events, delegates, and the Task Parallel Library.

## Deep Explanation

Message Passing is a fundamental concept in concurrent programming, where multiple processes or threads execute independently and communicate with each other to exchange information or coordinate their activities. The Message Passing technique enables these independent processes or threads to share data or synchronize their execution by sending and receiving messages through a communication channel.

In C# and .NET, Message Passing can be implemented using various constructs such as events, delegates, and the Task Parallel Library. For example, events and delegates can be used to implement the Observer pattern, where one object sends notifications to multiple other objects about its state changes. Similarly, the Task Parallel Library provides constructs like Channels, which provide a way to send and receive messages between different threads.

## Examples

Here's an example of Message Passing in C# using the Task Parallel Library:

```C#
using System;
using System.Threading.Channels;
using System.Threading.Tasks;

class Program
{
    static async Task Main(string[] args)
    {
        // Create a channel to send and receive messages
        var channel = Channel.CreateUnbounded<string>();

        // Start two tasks to send and receive messages
        var senderTask = Task.Run(async () =>
        {
            for (int i = 1; i <= 10; i++)
            {
                await channel.Writer.WriteAsync($"Message {i}");
            }

            channel.Writer.Complete();
        });

        var receiverTask = Task.Run(async () =>
        {
            await foreach (var message in channel.Reader.ReadAllAsync())
            {
                Console.WriteLine($"Received: {message}");
            }
        });

        // Wait for both tasks to complete
        await Task.WhenAll(senderTask, receiverTask);
    }
}
```

In this example, we create a Channel that can be used to send and receive messages of type string. We then start two tasks, one to send messages to the channel and the other to receive messages from the channel. The sender task writes ten messages to the channel and then completes it, while the receiver task waits for messages and prints them to the console. Finally, we wait for both tasks to complete using the Task.WhenAll method.

Here is a code example of implementing the Observer pattern using events and delegates in C#:

```C#
using System;

// Define the EventArgs class for passing data with the event
public class StockChangedEventArgs : EventArgs
{
    public string StockName { get; set; }
    public decimal Price { get; set; }

    public StockChangedEventArgs(string stockName, decimal price)
    {
        StockName = stockName;
        Price = price;
    }
}

// Define the subject (observable) class
public class StockTicker
{
    // Declare an event using the EventHandler delegate
    public event EventHandler<StockChangedEventArgs> StockChanged;

    // Simulate updating the stock prices
    public void UpdateStockPrice(string stockName, decimal price)
    {
        Console.WriteLine($"Updating {stockName} price to {price}");

        // Raise the event with the new data
        StockChanged?.Invoke(this, new StockChangedEventArgs(stockName, price));
    }
}

// Define the observer (subscriber) class
public class StockTickerDisplay
{
    private readonly string _name;

    public StockTickerDisplay(string name)
    {
        _name = name;
    }

    // Define a method that will handle the event
    public void OnStockChanged(object sender, StockChangedEventArgs e)
    {
        Console.WriteLine($"{_name} received update for {e.StockName}: {e.Price}");
    }
}

// Example usage
class Program
{
    static void Main(string[] args)
    {
        // Create the subject (observable) object
        var stockTicker = new StockTicker();

        // Create two observer (subscriber) objects
        var display1 = new StockTickerDisplay("Display 1");
        var display2 = new StockTickerDisplay("Display 2");

        // Subscribe the observer objects to the subject object's event
        stockTicker.StockChanged += display1.OnStockChanged;
        stockTicker.StockChanged += display2.OnStockChanged;

        // Update the stock prices (this will trigger the event)
        stockTicker.UpdateStockPrice("MSFT", 300.00m);
        stockTicker.UpdateStockPrice("AAPL", 400.00m);

        // Unsubscribe one of the observer objects
        stockTicker.StockChanged -= display2.OnStockChanged;

        // Update the stock prices again (this will only trigger one event handler)
        stockTicker.UpdateStockPrice("GOOG", 1500.00m);
    }
}
```

In this example, the StockTicker class is the subject (observable) and the StockTickerDisplay class is the observer (subscriber). The StockTicker class has an event named StockChanged that is used to notify subscribers of changes to the stock prices. The StockTickerDisplay class has a method named OnStockChanged that will be called when the StockChanged event is raised. The Main method creates the subject object, two observer objects, and subscribes the observer objects to the subject object's event. It then updates the stock prices twice, triggering the event handlers for both observer objects on the first update and only the event handler for one observer object on the second update.