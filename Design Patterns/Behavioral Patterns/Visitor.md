# Visitor

![](../Assets/visitor.png)

## Simple Explanation

The Visitor design pattern is a behavioral design pattern that allows you to add new operations to existing classes without modifying them. It separates the algorithm from the object structure it operates on, promoting loose coupling and making it easy to add new operations without affecting the existing classes.

## Deep Explanation

The Visitor pattern involes four main components:

1. Visitor - an interface or abstract class that defines a common interface for all concrete visitor classes. It declares a visit method for each concrete element class in the object structure.

2. Concrete Visitors - classes implementing the Visitor interface, encapsulating the behavior for a specific operation on the elements.

3. Element - an interface or abstract class that defines a common interface for all concrete element classes. It declares an accept method for receiving visitors.

4. Concrete Elements - classes implementing the Element interface, representing the objects that the visitor operates on.

## Examples

Let's imagine we have a simple object structure representing geometric shapes (circles and rectangles) and we want to perform different operations on them, such as drawing and calculating the area.

1. Create the Element and Visitor interfaces:

```C#
public interface IShape
{
    void Accept(IShapeVisitor visitor);
}

public interface IShapeVisitor
{
    void Visit(Circle circle);
    void Visit(Rectangle rectangle);
}
```

2. Implement Concrete Element classes:

```C#
public class Circle : IShape
{
    public int Radius { get; set; }

    public Circle(int radius)
    {
        Radius = radius;
    }

    public void Accept(IShapeVisitor visitor)
    {
        visitor.Visit(this);
    }
}

public class Rectangle : IShape
{
    public int Width { get; set; }
    public int Height { get; set; }

    public Rectangle(int width, int height)
    {
        Width = width;
        Height = height;
    }

    public void Accept(IShapeVisitor visitor)
    {
        visitor.Visit(this);
    }
}
```

3. Implement Concrete Visitor classes:

```C#
public class DrawVisitor : IShapeVisitor
{
    public void Visit(Circle circle)
    {
        Console.WriteLine($"Drawing a circle with radius {circle.Radius}.");
    }

    public void Visit(Rectangle rectangle)
    {
        Console.WriteLine($"Drawing a rectangle with width {rectangle.Width} and height {rectangle.Height}.");
    }
}

public class AreaVisitor : IShapeVisitor
{
    public void Visit(Circle circle)
    {
        double area = Math.PI * Math.Pow(circle.Radius, 2);
        Console.WriteLine($"The area of the circle is {area}");
    }

    public void Visit(Rectangle rectangle)
    {
        int area = rectangle.Width * rectangle.Height;
        Console.WriteLine($"The area of the rectangle is {area}");
    }
}
```

4. Use the Visitors and Elements:

```C#
class Program
{
    static void Main(string[] args)
    {
        var shapes = new IShape[]
        {
            new Circle(5),
            new Rectangle(10, 20)
        };

        var drawVisitor = new DrawVisitor();
        var areaVisitor = new AreaVisitor();

        foreach (IShape shape in shapes)
        {
            shape.Accept(drawVisitor);
            shape.Accept(areaVisitor);
        }
    }
}
```

In this example, `IShape` is the Element interface, `Circle` and `Rectangle` are Concrete Element classes, `IShapeVisitor` is the Visitor interface, and `DrawVisitor` and `AreaVisitor` are Concrete Visitor classes.

The `Circle` and `Rectangle` classes implement the `IShape` interface and represent the objects that the visitor operates on.

The `DrawVisitor` and `AreaVisitor` classes implement the `IShapeVisitor` interface, encapsulating the behavior for drawing shapes and calculating their areas, respectively.

The `IShape` interface declares an `Accept` method for receiving visitors, and the concrete element classes implement this method by calling the appropriate `Visit` method on the visitor. The `IShapeVisitor` interface declares separate `Visit` methods for each concrete element class, allowing the visitor to perform different operations on each element type.

By using the Visitor pattern, you can create a flexible and modular system that allows you to add new operations to existing classes without modifying them. This promotes loose coupling and improves code maintainability.