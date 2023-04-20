# Template Method

![](../Assets/template-method.png)

## Simple Explanation

The Template Method is a behavioral design pattern that defines the "skeleton" of an algorithm in a base class, allowing subclasses to override certain steps without changing the algorithm's structure. It promotes code reusability and reduces code duplication.

## Deep Explanation

The Template Method pattern involves two main components:

1. Abstract Base Class - contains the template method that defines the skeleton of the algorithm. It may also implement some default behavior for some or all steps of the algorithm. **The Template Method should be declared sealed (or final in other languages) to prevent subclasses from overriding it**.

2. Concrete Subclasses - extend the abstract base class and override specific steps of the algorithm to provide their own implementation.

## Examples

Let's imagine we have a simple data exporter utility that exports data to different formats such as CSV and XML.

1. Create the Abstract Base Class:

```C#
public abstract class DataExporter
{
    public sealed void ExportData()
    {
        GetDataFromDatabase();
        ProcessData();
        SaveDataToFile();
    }

    protected abstract void GetDataFromDatabase();
    protected abstract void ProcessData();
    protected abstract void SaveDataToFile();
}
```

2. Implement Concrete Subclasses:

```C#
public class CsvDataExporter : DataExporter
{
    protected override void GetDataFromDatabase()
    {
        Console.WriteLine("Getting data from the database for CSV export.");
    }

    protected override void ProcessData()
    {
        Console.WriteLine("Processing data for CSV export.");
    }

    protected override void SaveDataToFile()
    {
        Console.WriteLine("Saving data to a CSV file.");
    }
}

public class XmlDataExporter : DataExporter
{
    protected override void GetDataFromDatabase()
    {
        Console.WriteLine("Getting data from the database for XML export.");
    }

    protected override void ProcessData()
    {
        Console.WriteLine("Processing data for XML export.");
    }

    protected override void SaveDataToFile()
    {
        Console.WriteLine("Saving data to a XML file.");
    }
}
```

3. Use the Template Method and Concrete Subclasses:

```C#
class Program
{
    static void Main(string[] args)
    {
        var csvExporter = new CsvDataExporter();
        csvExporter.ExportData();

        var xmlExporter = new XmlDataExporter();
        xmlExporter.ExportData();
    }
}
```

In this example, `DataExporter` is the Abstract Base Class, and `CsvDataExporter` and `XmlDataExporter` are Concrete Subclasses. The `DataExporter` defines the `ExportData` template method, which outlines the structure of the algorithm. The concrete subclasses provide their own implementation for each step of the algorithm.

By using the Template Method pattern, you can create a flexible and modular system that allows subclasses to provide their own implementation for certain steps of an algorithm without affecting the overall structure. This promotes code reusability and reduces code duplication.