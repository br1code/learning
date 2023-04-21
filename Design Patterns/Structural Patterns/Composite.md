# Composite

![](../Assets/composite.png)

## Simple Explanation

The Composite design pattern is a structural pattern that allows you to treat a group of objects as a single object. It helps to simplify operations on tree structures by treating individual objects (leaf nodes) and compositions of objects (branches) uniformly.

## Deep Explanation

The Composite pattern involves three main components:

1. Component - an abstract base class or interface that defines the common methods for both leaf nodes and branches.

2. Leaf - represents individual objects that do not have children. It implements the Component interface.

3. Composite - represents a group of objects (either Leaf or other Composite objects). It also implements the 
Component interface and maintains a list of child Components.

By using the Composite pattern, clients can interact with the tree structure through the Component interface, without worrying about whether they are dealing with individual objects (Leaf) or compositions of objects (Composite).

## Examples

Let's imagine we have a simple application that manages a hierarchy of files and folders.

1. Create the Component interface:

```C#
public interface IFileSystemComponent
{
    void Display(int indent = 0);
}
```

2. Implement the Leaf class:

```C#
public class File : IFileSystemComponent
{
    private readonly string _name;

    public File(string name)
    {
        _name = name;
    }

    public void Display(int indent = 0)
    {
        Console.WriteLine($"{new string(' ', indent)}- {_name}");
    }
}
```

3. Implement the Composite class:

```C#
public class Folder : IFileSystemComponent
{
    private readonly string _name;
    private readonly List<IFileSystemComponent> _components = new();

    public Folder(string name)
    {
        _name = name;
    }

    public void AddComponent(IFileSystemComponent component)
    {
        _components.Add(component);
    }

    public void RemoveComponent(IFileSystemComponent component)
    {
        _components.Remove(component);
    }

    public void Display(int indent = 0)
    {
        Console.WriteLine($"{new string(' ', indent)}+ {_name}");

        foreach (var component in _components)
        {
            component.Display(indent + 2);
        }
    }
}
```

4. Use the Composite pattern:

```C#
class Program
{
    static void Main(string[] args)
    {
        Folder root = new("root");
        File file1 = new("file1.txt");
        File file2 = new("file2.txt");

        root.AddComponent(file1);
        root.AddComponent(file2);

        Folder subFolder = new("subfolder");
        File file3 = new("file3.txt");
        subFolder.AddComponent(file3);

        root.AddComponent(subFolder);

        root.Display();
    }
}
```

In this example, `IFileSystemComponent` is the Component interface, `File` is the Leaf class, and `Folder` is the Composite class. The `IFileSystemComponent` interface defines a `Display` method that is used to display the structure of files and folders.

The `File` class represents individual files and implements the `IFileSystemComponent` interface. The `Folder` class represents a group of files or folders and also implements the `IFileSystemComponent` interface. It maintains a list of child components and provides methods for adding and removing them.

In the `Main` method of the `Program` class, we create a simple hierarchy of files and folders using the Composite pattern. We can easily display the entire structure by calling the `Display` method on the root folder, which in turn calls the `Display` method on all child components recursively.

This example demonstrates how the Composite pattern can be used to simplify operations on tree structures by treating individual objects and compositions of objects uniformly.