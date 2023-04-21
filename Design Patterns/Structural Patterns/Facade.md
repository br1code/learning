# Facade

![](../Assets/facade.png)

## Simple Explanation

The Facade design pattern is a structural pattern that provides a simplified interface to a more complex subsystem, making it easier to use and understand. It serves as a "wrapper" or "facade" that hides the complexity of the underlying system by providing a unified interface to interact with it.

## Deep Explanation

The Facade pattern is particularly useful when dealing with a complex subsystem that consists of multiple classes with intricate dependencies and interactions. By providing a simplified interface, clients can interact with the subsystem without needing to know the internal details or complexities.

The Facade pattern can also help to reduce coupling between the subsystem and its clients, as the clients only need to depend on the facade rather than individual subsystem components. This promotes better separation of concerns and makes the system more maintainable and extensible.

## Examples

Consider an example of an e-commerce system that has multiple subsystems like order processing, inventory management, and shipping. To simplify the interaction between the client and these subsystems, we can create a facade that provides a straightforward interface for placing an order.

1. Define subsystem classes:

```C#
public class OrderProcessing
{
    public void ProcessOrder()
    {
        Console.WriteLine("Processing order...");
    }
}

public class InventoryManagement
{
    public void UpdateInventory()
    {
        Console.WriteLine("Updating inventory...");
    }
}

public class Shipping
{
    public void ShipOrder()
    {
        Console.WriteLine("Shipping order...");
    }
}
```

2. Create the Facade class:

```C#
public class OrderFacade
{
    private readonly OrderProcessing _orderProcessing;
    private readonly InventoryManagement _inventoryManagement;
    private readonly Shipping _shipping;

    public OrderFacade()
    {
        _orderProcessing = new OrderProcessing();
        _inventoryManagement = new InventoryManagement();
        _shipping = new Shipping();
    }

    public void PlaceOrder()
    {
        _orderProcessing.ProcessOrder();
        _inventoryManagement.UpdateInventory();
        _shipping.ShipOrder();
    }
}
```

3. Use the Facade pattern:

```C#
class Program
{
    static void Main(string[] args)
    {
        OrderFacade orderFacade = new OrderFacade();
        orderFacade.PlaceOrder();
    }
}
```

In this example, `OrderProcessing`, `InventoryManagement`, and `Shipping` are the subsystem classes that handle different aspects of an e-commerce system. The `OrderFacade` class is the facade that simplifies the interaction with these subsystems by providing a single `PlaceOrder` method.

The client code in the `Program` class only needs to interact with the `OrderFacade` and call the `PlaceOrder` method to place an order, without needing to know the details of order processing, inventory management, or shipping.

By using the Facade pattern, we can make it easier for clients to interact with complex subsystems, reduce coupling, and promote better separation of concerns.