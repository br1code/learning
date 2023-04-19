# Boxing vs Unboxing

## Explanation

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

## Bonus: Explained as if I were 5

Imagine you have a toy box with different toys inside. Some toys are big and some toys are small, but they all fit inside the box. When you want to play with a toy, you take it out of the box and start playing with it. When you're done playing with it, you put it back in the box.

In programming, we also have two boxes called the stack and the heap. The stack is a small box where we put small things like numbers, and the heap is a big box where we put bigger things like objects.

When we want to use a number, we put it in the stack. But sometimes we want to use a number as an object, like if we want to add some special behavior to it. To do this, we put the number into the heap box and make it into an object. This is called boxing.

When we're done using the object, we can take it out of the heap box and put it back in the stack box as a regular number. This is called unboxing.

**So, in short, boxing is when we turn a value type (like a number) into a reference type (like an object) so that we can use it with more functionality. And unboxing is when we turn that reference type back into a value type so that we can use it as a regular value again.**