# Generics

## Simple Explanation:

Generics in C# allow you to create classes, interfaces, methods, and structures that work with different data types, while providing type safety at compile time. This means you can write code that is more flexible and reusable, without sacrificing performance or safety.

## Deep Explanation:

Generics in C# were introduced in .NET Framework 2.0 as a way to enable type-safe programming with parameters of different data types. With generics, you can write code that works with a variety of data types, without having to create separate methods or classes for each type.

Generics use type parameters to specify the data type that will be used in a particular method or class. These type parameters are defined using angle brackets (<>) and can be any valid identifier. When the code is compiled, the compiler generates a separate class or method for each unique combination of type parameters used.

Generics provide several benefits over non-generic code, including type safety, improved performance, and code reusability. By enforcing type safety at compile time, generics help prevent runtime errors caused by invalid data types. This can improve the reliability and stability of your code.

Generics also improve performance by reducing the need for type conversions and boxing/unboxing operations. This can lead to faster code execution and reduced memory usage.

Finally, generics promote code reusability by allowing you to write code that can be used with different data types. This can reduce the amount of code you need to write and maintain, and make your code more flexible and extensible.

## Examples

### Generic Classes

In this example, we define a generic class called Stack that can work with any data type. The type parameter T is used to represent the data type that will be stored in the stack. We use a List<T> to store the items in the stack.

```C#
public class Stack<T>
{
    private List<T> items = new List<T>();
    
    public void Push(T item)
    {
        items.Add(item);
    }
    
    public T Pop()
    {
        T item = items[items.Count - 1];
        items.RemoveAt(items.Count - 1);
        return item;
    }
    
    public int Count
    {
        get { return items.Count; }
    }
}
```

### Generic Methods:

In this example, we define a generic method called Max that can work with any data type that implements the IComparable<T> interface. The type parameter T is used to represent the data type of the parameters and return value. We use the CompareTo method to compare the two values and determine which one is greater.

```C#
public static T Max<T>(T first, T second) where T : IComparable<T>
{
    if (first.CompareTo(second) > 0)
    {
        return first;
    }
    else
    {
        return second;
    }
}
```

### Using Generics:


```C#
Stack<int> intStack = new Stack<int>();
intStack.Push(10);
intStack.Push(20);
intStack.Push(30);

Console.WriteLine("Count: {0}", intStack.Count);
Console.WriteLine("Pop: {0}", intStack.Pop());
Console.WriteLine("Count: {0}", intStack.Count);

Stack<string> stringStack = new Stack<string>();
stringStack.Push("hello");
stringStack.Push("world");

Console.WriteLine("Count: {0}", stringStack.Count);
Console.WriteLine("Pop: {0}", stringStack.Pop());
Console.WriteLine("Count: {0}", stringStack.Count);

```

```C#
int maxInt = Max<int>(10, 20);
double maxDouble = Max<double>(1.5);
string maxStr = Max<string>("hello", "world");
```