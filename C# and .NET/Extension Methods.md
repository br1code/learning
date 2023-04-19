# Extension Methods

## Simple Explanation

Extension methods in C# allow you to add new methods to existing types without having to derive a new type or modify the original type. This means that you can extend classes, structs, and interfaces that you did not create and add new methods to them as if they were part of the original definition.

## Deep Explanation

Extension methods in C# allow developers to add methods to a class or interface that they did not create or modify the original source code. This is done by defining a new static method and tagging it with the "this" keyword before the first parameter, which specifies the class or interface that you want to extend. When you do this, the extension method becomes available as an instance method of the specified class or interface.

Extension methods can be used to add new functionality to existing types without modifying them. This means that you can add new methods to the .NET Framework classes or third-party libraries, without modifying the source code.

Extension methods can also be used to improve the readability and maintainability of your code. For example, if you have a complex algorithm that is used throughout your application, you can create an extension method that encapsulates that algorithm and makes it easier to reuse.

## Examples

Let's say you want to add a new method to the built-in string type in C#. You can do this using an extension method like so:

```C#
public static class StringExtensions
{
    public static string Reverse(this string input)
    {
        char[] chars = input.ToCharArray();
        Array.Reverse(chars);
        return new string(chars);
    }
}
```

Now, you can use the new method as if it were part of the string type:

```C#
string input = "hello world";
string reversed = input.Reverse();
Console.WriteLine(reversed); // Output: "dlrow olleh"
```