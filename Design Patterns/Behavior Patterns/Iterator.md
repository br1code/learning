# Iterator

## Simple Explanation

The Iterator design pattern is a behavioral design pattern that provides a standardized way to traverse a collection of objects without exposing the underlying implementation. It allows you to decouple the traversal logic from the collection itself, making your code more modular and easier to maintain.

## Deep Explanation

The Iterator pattern consists of two main components:

1. Iterator - an interface or abstract class that defines methods for traversing a collection (e.g., HasNext, Next).

2. Concrete Iterator - a class implementing the Iterator interface, which contains the specific traversal logic for a particular collection.

By implementing the Iterator pattern, you can enable a uniform way of accessing elements in different types of collections, such as lists, arrays, trees, or graphs. This makes it easier to work with various data structures and simplifies the process of adding new types of collections to your application.

## Examples

In C# and .NET, the Iterator pattern is already implemented through the `IEnumerable` and `IEnumerator` interfaces. Here's an example of how to use these interfaces to create a custom collection with an iterator:

1. Create a custom collection implementing `IEnumerable`:

```C#
public class CustomCollection<T> : IEnumerable<T>
{
    private readonly List<T> _items = new List<T>();

    public void Add(T item)
    {
        _items.Add(item);
    }

    public IEnumerator<T> GetEnumerator()
    {
        return new CustomIterator<T>(_items);
    }

    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }
}
```

2. Implement a Concrete Iterator class:

```C#
public class CustomIterator<T> : IEnumerator<T>
{
    private readonly List<T> _items;
    private int _currentIndex = -1;

    public CustomIterator(List<T> items)
    {
        _items = items;
    }

    public T Current => _items[_currentIndex];

    object IEnumerator.Current => Current;

    public void Dispose() { }

    public bool MoveNext()
    {
        _currentIndex++;
        return _currentIndex < _items.Count;
    }

    public void Reset()
    {
        _currentIndex = -1;
    }
}
```

3. Use the custom collection and iterator:

```C#
class Program
{
    static void Main(string[] args)
    {
        CustomCollection<string> fruits = new CustomCollection<string>();
        fruits.Add("Apple");
        fruits.Add("Banana");
        fruits.Add("Orange");

        // Iterate over the custom collection
        foreach (string fruit in fruits)
        {
            Console.WriteLine(fruit);
        }
    }
}
```

In this example, `CustomCollection` is a custom collection implementing the `IEnumerable` interface, while `CustomIterator` is a concrete iterator implementing the `IEnumerator` interface. The `foreach` loop in the `Main` method demonstrates how to use the iterator to traverse the custom collection.

By using the Iterator pattern, you can enable consistent traversal of different types of collections and make it easier to add new collections to your application without changing the traversal logic.