# Stack vs Heap

## Explanation

In C# and .NET, memory is allocated in two different ways: the stack and the heap.

The stack is a region of memory used for storing local variables and function call information. Each time a function is called, a new stack frame is created on top of the previous frame. When the function returns, the stack frame is removed. This allows for quick memory allocation and deallocation, as the stack is managed automatically by the system.

## Examples

Here is an example of how the stack is used in C#:

```C#
void Foo()
{
    int x = 10; // x is stored on the stack
    Bar(x); // call another function
    // x is automatically deallocated when this function returns
}

void Bar(int y)
{
    int z = y + 5; // z is stored on the stack
    // z is automatically deallocated when this function returns
}

```

The heap is a region of memory used for dynamic memory allocation, such as for objects or arrays. Memory on the heap must be explicitly allocated and deallocated by the programmer, using keywords like new and delete. The heap can be slower than the stack, as the allocation and deallocation of memory must be managed manually.

Here is an example of how the heap is used in C#:

```C#
void Baz()
{
    int[] arr = new int[5]; // allocate memory on the heap for an array of 5 integers
    // use the array
    // must explicitly deallocate the memory when finished
    arr = null; // set the reference to null to allow garbage collection to deallocate the memory
}

```

## Bonus: Explained like I was 5

Imagine you have a big box of toys. When you play with your toys, you take them out of the box and put them on the floor. When you're done playing with a toy, you put it back in the box.

Your computer works kind of like your toy box. It has a big box (called memory) where it stores things it needs to use. But instead of toys, it stores information like numbers and words.

The stack is like a little tray that sits on top of the memory box. When your computer needs to remember something, it puts it on the tray (the stack). When it's done with that thing, it takes it off the tray.

The heap is like the rest of the space in the memory box. When your computer needs to remember something big, it puts it in the heap. The heap is bigger than the stack, so it can hold bigger things.

**So, the stack is for little things that your computer doesn't need for very long, and the heap is for big things that your computer needs to use for a while.**