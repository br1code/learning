# Strategy

![](../Assets/strategy.png)

## Simple Explanation

The Strategy design pattern is a behavioral design pattern that defines a family of interchangeable algorithms and enables the selection of an algorithm at runtime. It promotes loose coupling and makes it easy to switch between different algorithms without modifying the context or the algorithms themselves.

## Deep Explanation

The Strategy pattern involves three main components:

1. Context - the class that uses the strategy object to perform a specific operation. It maintains a reference to a strategy object and delegates the operation to it.

2. Strategy - an interface or abstract class that defines a common interface for all concrete strategy classes. It declares the method for performing the operation.

3. Concrete Strategies - classes implementing the Strategy interface, encapsulating the behavior or a specific algorithm.

By implementing the Strategy pattern, you can organize related algorithms into separate classes and make it easy to extend the code with new algorithms without modifying the context or other algorithms.

## Examples

Let's imagine we have a simple compression utility that supports different compression algorithms, such as ZIP and RAR.

1. Create the Strategy interface:

```C#
public interface ICompressionStrategy
{
    void Compress(string inputFile, string outputFile);
}
```

2. Implement Concrete Strategy classes:

```C#
public class ZipCompressionStrategy : ICompressionStrategy
{
    public void Compress(string inputFile, string outputFile)
    {
        Console.WriteLine($"Compressing '{inputFile}' to '{outputFile}' using ZIP algorithm.");
    }
}

public class RarCompressionStrategy : ICompressionStrategy
{
    public void Compress(string inputFile, string outputFile)
    {
        Console.WriteLine($"Compressing '{inputFile}' to '{outputFile}' using RAR algorithm.");
    }
}
```

3. Implement the Context class:

```C#
public class CompressionUtility
{
    private ICompressionStrategy _strategy;

    public CompressionUtility(ICompressionStrategy strategy)
    {
        _strategy = strategy;
    }

    public void SetStrategy(ICompressionStrategy strategy)
    {
        _strategy = strategy;
    }

    public void Compress(string inputFile, string outputFile)
    {
        _strategy.Compress(inputFile, outputFile);
    }
}
```

4. Use the Context and Strategy objects:

```C#
class Program
{
    static void Main(string[] args)
    {
        var utility = new CompressionUtility(new ZipCompressionStrategy());
        utility.Compress("file.txt", "file.zip");

        utility.SetStrategy(new RarCompressionStrategy());
        utility.Compress("file.txt", "file.rar");
    }
}
```

In this example, `CompressionUtility` is the Context, and `ZipCompressionStrategy` and `RarCompressionStrategy` are Concrete Strategy classes. The `CompressionUtility` delegates the compression operation to the strategy object, allowing it to switch between different compression algorithms at runtime.

By using the Strategy pattern, you can create a flexible and modular system that allows the context to select an algorithm at runtime without affecting other parts of the code. This promotes loose coupling and improves code maintainability.