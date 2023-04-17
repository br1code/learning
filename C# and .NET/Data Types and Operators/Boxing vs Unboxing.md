# Boxing vs Unboxing

In C# and .NET, boxing and unboxing are two operations that are used to convert between value types and reference types.

Boxing is the process of converting a value type, such as an int or float, to an object of type System.Object. When a value type is boxed, a new object is created on the heap to hold the value, and a reference to that object is returned. Boxing is useful when you need to pass a value type to a method that expects an object reference, such as when using collections like List<object> or when passing parameters to reflection APIs.

Here is an example of how boxing works in C#:


```C#
int x = 42; // a value type
object obj = x; // box x into an object

```
In this example, x is a value type, and obj is a reference type. When x is assigned to obj, it is automatically boxed into an object of type System.Object.

Unboxing is the process of extracting a value type from an object reference. When an object is unboxed, the value type is copied from the object on the heap back to the stack. Unboxing is necessary when you need to retrieve the original value of a boxed value type, such as when iterating through a collection of boxed values.

Here is an example of how unboxing works in C#:

```C#
object obj = 42; // an object reference
int x = (int)obj; // unbox obj back to int
```

In this example, obj is an object reference that contains a boxed int value. When obj is cast back to int, the value is unboxed and copied back onto the stack.

It's important to note that boxing and unboxing can have performance implications, as creating and copying objects on the heap can be slower than working with value types directly on the stack. It's generally a good practice to avoid unnecessary boxing and unboxing operations in performance-critical code.