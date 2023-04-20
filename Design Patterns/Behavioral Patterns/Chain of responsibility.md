# Chain of responsability

![](../Assets/chain-of-responsibility.png)

## Simple Explanation

The Chain of Responsibility design pattern is a behavioral design pattern that allows you to decouple the sender of a request from its receiver by allowing multiple objects (handlers) to handle the request. The request is passed along the chain of handlers until one of them handles it or the chain ends.

## Deep Explanation

In the Chain of Responsibility pattern, each handler in the chain can either handle the request or pass it along to the next handler in the chain. This provides a way to manage the execution order of handlers and can help improve the separation of concerns in your code.

A chain of responsibility typically consists of an abstract base handler class or an interface, which defines the common methods for handling requests, and concrete handler classes that implement the handling logic. The concrete handlers can be configured and combined as needed to create the desired chain.

## Examples

Imagine we have an application that processes different types of files. We can create a file handler chain to process these files.

1. Create an abstract base handler class or an interface:

```C#
public interface IFileHandler
{
    IFileHandler NextHandler { get; set; }
    void ProcessFile(string fileName);
}
```

2. Implement concrete handler classes:

```C#
public class TextFileHandler : IFileHandler
{
    public IFileHandler NextHandler { get; set; }

    public void ProcessFile(string fileName)
    {
        if (Path.GetExtension(fileName).Equals(".txt"))
        {
            // Process text file
            Console.WriteLine("Processing text file: " + fileName);
        }
        else if (NextHandler != null)
        {
            NextHandler.ProcessFile(fileName);
        }
    }
}

public class PdfFileHandler : IFileHandler
{
    public IFileHandler NextHandler { get; set; }

    public void ProcessFile(string fileName)
    {
        if (Path.GetExtension(fileName).Equals(".pdf"))
        {
            // Process PDF file
            Console.WriteLine("Processing PDF file: " + fileName);
        }
        else if (NextHandler != null)
        {
            NextHandler.ProcessFile(fileName);
        }
    }
}
```

3. Create and use the chain of handlers:

```C#
class Program
{
    static void Main(string[] args)
    {
        IFileHandler textFileHandler = new TextFileHandler();
        IFileHandler pdfFileHandler = new PdfFileHandler();

        // Configure the chain
        textFileHandler.NextHandler = pdfFileHandler;

        // Process files
        textFileHandler.ProcessFile("document.txt");
        textFileHandler.ProcessFile("report.pdf");
    }
}
```

In this example, `TextFileHandler` and `PdfFileHandler` are concrete handlers implementing the `IFileHandler` interface. When the `ProcessFile` method is called on the first handler in the chain, it checks if it can process the file. If not, it passes the request to the next handler in the chain. This continues until a handler processes the file or the chain ends.