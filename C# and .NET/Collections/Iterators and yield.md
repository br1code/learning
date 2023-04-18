# Iterators and yield

## Simple Explanation

Iterators allow you to write code that can traverse a collection of items one at a time without having to load the entire collection into memory at once. 

The yield keyword is used to define an iterator method, which returns an IEnumerable or IEnumerator object that can be used to iterate over a collection.

## Deep Explanation

Iterators are a way to generate a sequence of values in a deferred manner. In other words, the values are only calculated and returned as needed, rather than all at once. This is useful for working with large or infinite sequences, where loading all the values into memory at once would be impractical or impossible.

To create an iterator method, you use the yield keyword to return each value in the sequence. The method itself returns an IEnumerable or IEnumerator object that can be used to iterate over the sequence. Each time the yield keyword is encountered, the value is returned and the method is suspended until the next value is requested.

Here's an example of an iterator method that returns the first n Fibonacci numbers:

```C#
public static IEnumerable<int> Fibonacci(int n)
{
    int a = 0, b = 1;

    for (int i = 0; i < n; i++)
    {
        yield return a;
        int c = a + b;
        a = b;
        b = c;
    }
}

foreach (int num in Fibonacci(10))
{
    Console.WriteLine(num);
}
```

The foreach loop will iterate over the IEnumerable object returned by the Fibonacci method, and each time it requests the next value, the method will generate the next number in the sequence using the yield keyword.

The yield keyword is used to define an iterator method. When the iterator method is called, it returns an IEnumerable or IEnumerator object that can be used to iterate over the sequence of values. The yield keyword tells the compiler to generate code to create an iterator state machine that can be used to iterate over the sequence of values.

When the iterator method is called, it executes the code up to the first yield statement. The value specified in the yield statement is returned to the calling code. The state of the iterator method is then saved, and the calling code can continue to iterate over the sequence of values by calling the MoveNext method on the IEnumerable or IEnumerator object. Each time the MoveNext method is called, the iterator method resumes execution from where it left off and returns the next value in the sequence.

## Examples

Example 1: Generating a sequence of integers

```C#
public static IEnumerable<int> GetNumbers()
{
    for (int i = 1; i <= 10; i++)
    {
        yield return i;
    }
}

foreach (int num in GetNumbers())
{
    Console.WriteLine(num);
}
// Output: 1 2 3 4 5 6 7 8 9 10
```

Example 2: Filtering a sequence of strings

```C#
public static IEnumerable<string> GetNamesStartingWithA(string[] names)
{
    foreach (string name in names)
    {
        if (name.StartsWith("A"))
        {
            yield return name;
        }
    }
}

string[] names = { "Alice", "Bob", "Alex", "Charlie" };
foreach (string name in GetNamesStartingWithA(names))
{
    Console.WriteLine(name);
}
// Output: Alice Alex
```

Example 3: Generating an infinite sequence of numbers

```C#
public static IEnumerable<int> GetInfiniteSequence()
{
    int i = 0;
    while (true)
    {
        yield return i++;
    }
}

foreach (int num in GetInfiniteSequence().Take(10))
{
    Console.WriteLine(num);
}
// Output: 0 1 2 3 4 5 6 7 8 9
```