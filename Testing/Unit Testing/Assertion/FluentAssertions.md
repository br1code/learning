# FluentAssertions

Assertions are an essential part of unit testing, as they verify that the actual output of a unit matches the expected output. If an assertion passes, it means the unit is functioning correctly under the given conditions. If it fails, it indicates a bug or a flaw in the unit's implementation. Assertions can be simple boolean expressions or more sophisticated methods provided by testing frameworks.

**FluentAssertions** is a popular .NET library that provides a rich set of assertion methods with a fluent interface, making it easy to write readable and expressive assertions. The library is compatible with various testing frameworks, including NUnit, xUnit, and MSTest.

Here's a brief overview of some of the primary assertion methods provided by FluentAssertions:

1. Be, NotBe: Used for asserting that a value is equal or not equal to an expected value.
2. BeTrue, BeFalse: Used for asserting that a boolean value is true or false.
3. BeOfType, NotBeOfType: Used for asserting that an object is of a specific type or not of a specific type.
4. BeEmpty, NotBeEmpty: Used for asserting that a collection is empty or not empty.
5. BeEquivalentTo: Used for asserting that two objects or collections are equivalent.
6. Contain, NotContain: Used for asserting that a collection contains or does not contain specific elements.
7. HaveCount: Used for asserting that a collection has a specific number of elements.

## Examples

Let's consider an example using C# and the FluentAssertions library. We will create a simple unit test for a method that calculates the sum of two numbers.

First, install the FluentAssertions NuGet package by executing the following command in your terminal or adding it through the NuGet Package Manager in Visual Studio:

```
Install-Package FluentAssertions
```

Next, create a class with a method to add two numbers:

```csharp
public class Calculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }
}
```

Now, create a test class using NUnit, xUnit, or MSTest, and import the FluentAssertions namespace:

```csharp
using FluentAssertions;
```

Write a test method to verify the functionality of the `Add` method:

```csharp
[Test] // NUnit attribute for a test method
public void Add_TwoNumbers_ShouldReturnCorrectSum()
{
    // Arrange
    var calculator = new Calculator();
    int a = 5;
    int b = 3;
    int expectedResult = 8;

    // Act
    int actualResult = calculator.Add(a, b);

    // Assert
    actualResult.Should().Be(expectedResult);
}
```

In this example, we used the `Be` method from FluentAssertions to assert that the result of the `Add` method is equal to the expected sum. If the test passes, it means the `Add` method is working correctly for the given inputs.

---

Let's consider a more complex real-world example, involving a simple e-commerce system with classes for a product, an order, and a shopping cart. We'll demonstrate different types of assertions using FluentAssertions in this context.

First, let's define the classes:

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public List<Product> Products { get; set; }
    public decimal TotalPrice => Products.Sum(product => product.Price);
}

public class ShoppingCart
{
    public List<Product> Products { get; set; }

    public void AddProduct(Product product)
    {
        Products.Add(product);
    }

    public Order Checkout()
    {
        var order = new Order { Id = 1, Products = Products };
        Products = new List<Product>();
        return order;
    }
}
```

Now, let's create test methods for the `ShoppingCart` class, using NUnit as the testing framework and FluentAssertions for the assertions.

1. Test adding a product to the shopping cart:

```csharp
[Test]
public void AddProduct_ValidProduct_ShouldAddToCart()
{
    // Arrange
    var shoppingCart = new ShoppingCart();
    var product = new Product { Id = 1, Name = "Book", Price = 20m };

    // Act
    shoppingCart.AddProduct(product);

    // Assert
    shoppingCart.Products.Should().Contain(product);
}
```

2. Test checking out and creating an order:

```csharp
[Test]
public void Checkout_WithProducts_ShouldCreateOrderAndEmptyCart()
{
    // Arrange
    var shoppingCart = new ShoppingCart();
    var product1 = new Product { Id = 1, Name = "Book", Price = 20m };
    var product2 = new Product { Id = 2, Name = "Laptop", Price = 800m };
    shoppingCart.AddProduct(product1);
    shoppingCart.AddProduct(product2);

    // Act
    var order = shoppingCart.Checkout();

    // Assert
    order.Should().NotBeNull();
    order.Products.Should().HaveCount(2)
        .And.Contain(product1)
        .And.Contain(product2);
    order.TotalPrice.Should().Be(820m);
    shoppingCart.Products.Should().BeEmpty();
}
```

3. Test checking out with an empty cart:

```csharp
[Test]
public void Checkout_EmptyCart_ShouldCreateEmptyOrder()
{
    // Arrange
    var shoppingCart = new ShoppingCart();

    // Act
    var order = shoppingCart.Checkout();

    // Assert
    order.Should().NotBeNull();
    order.Products.Should().BeEmpty();
    order.TotalPrice.Should().Be(0m);
}
```

In these examples, we used various FluentAssertions methods like `Contain`, `HaveCount`, `BeEmpty`, and `Be` to create expressive assertions for different scenarios in our e-commerce system. These tests help verify that our code works correctly for adding products to a shopping cart, checking out, and creating orders.

FluentAssertions provides many more assertion methods and options, which can be found in the official documentation: https://fluentassertions.com/introduction