# Factory Method

![](../Assets/factory-method.png)

## Simple Explanation

The Factory Method design pattern is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created. It promotes loose coupling between classes by delegating object creation to subclasses instead of using direct instantiation.

## Deep Explanation

The Factory Method pattern involves three main components:

1. Creator - an abstract class or interface that defines the factory method for creating objects. The factory method is usually declared as abstract or virtual, allowing subclasses to provide their own implementation.

2. Concrete Creator - a subclass of the Creator class, which implements the factory method to create and return a specific type of object.

3. Product - an interface or abstract class that defines a common interface for the objects the factory method creates. Concrete Product classes implement this interface.

By implementing the Factory Method pattern, you can create a flexible and modular system that allows subclasses to decide which objects to create, without specifying the concrete classes in the superclass. This promotes loose coupling and improves code maintainability.

## Examples

Let's imagine we have a simple application that creates and processes different types of documents.

1. Create the Product and Creator interfaces:

```C#
public interface IDocument
{
    void Process();
}

public abstract class DocumentCreator
{
    public abstract IDocument CreateDocument();
}
```

2. Implement Concrete Product classes:

```C#
public class WordDocument : IDocument
{
    public void Process()
    {
        Console.WriteLine("Processing a Word document.");
    }
}

public class PdfDocument : IDocument
{
    public void Process()
    {
        Console.WriteLine("Processing a PDF document.");
    }
}
```

3. Implement Concrete Creator classes:

```C#
public class WordDocumentCreator : DocumentCreator
{
    public override IDocument CreateDocument()
    {
        return new WordDocument();
    }
}

public class PdfDocumentCreator : DocumentCreator
{
    public override IDocument CreateDocument()
    {
        return new PdfDocument();
    }
}
```

4. Use the Factory Method:

```C#
class Program
{
    static void Main(string[] args)
    {
        DocumentCreator[] creators = new DocumentCreator[]
        {
            new WordDocumentCreator(),
            new PdfDocumentCreator()
        };

        foreach (DocumentCreator creator in creators)
        {
            IDocument document = creator.CreateDocument();
            document.Process();
        }
    }
}
```

In this example, `IDocument` is the Product interface, `WordDocument` and `PdfDocument` are Concrete Product classes, `DocumentCreator` is the Creator interface, and `WordDocumentCreator` and `PdfDocumentCreator` are Concrete Creator classes. The `DocumentCreator` interface declares an abstract `CreateDocument` method for creating objects, and the concrete creator classes implement this method to create specific types of documents.

By using the Factory Method pattern, you can create a flexible and modular system that allows you to add new types of documents without modifying the existing code that processes them. This promotes loose coupling and improves code maintainability.