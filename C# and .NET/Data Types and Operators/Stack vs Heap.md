# Stack vs Heap

In C# and .NET, memory is allocated in two different ways: the stack and the heap.

The stack is a region of memory used for storing local variables and function call information. Each time a function is called, a new stack frame is created on top of the previous frame. When the function returns, the stack frame is removed. This allows for quick memory allocation and deallocation, as the stack is managed automatically by the system.

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

The Garbage Collector (GC) in the CLR is responsible for automatically managing memory on the heap that has been allocated by the programmer. When an object is created with the new keyword, memory is allocated on the heap to store the object's data. When the object is no longer being used, the GC is responsible for freeing up that memory by performing garbage collection. The GC periodically scans the heap for objects that are no longer being referenced by any part of the application, and frees up the memory used by those objects. This is known as "mark and sweep" garbage collection.