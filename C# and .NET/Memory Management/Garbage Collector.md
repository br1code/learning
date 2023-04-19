# Garbage Collector

## Simple Explanation

Garbage Collector (GC) is a built-in feature of the .NET Framework that automatically manages the memory used by a .NET application. It automatically detects and frees up memory that is no longer being used by the application. The GC performs this by periodically scanning the memory used by the application and identifying objects that are no longer being used. Once these objects are identified, the GC frees up the memory they are using.

## Deep Explanation

In .NET, memory allocation and deallocation are managed by the Garbage Collector (GC). The GC is responsible for identifying memory that is no longer being used by the application and freeing it up for future use.

When an object is created in .NET, the memory is allocated from the managed heap, which is a contiguous block of memory that is reserved for the use of the .NET application. Once an object is no longer being used by the application, it becomes eligible for garbage collection. The GC periodically scans the managed heap for objects that are no longer being used and frees up the memory they are using.

The GC uses a number of different algorithms to determine which objects are eligible for garbage collection and when to free up memory. Some of the key algorithms used by the GC include:

- Mark and sweep: This algorithm involves scanning the managed heap to identify objects that are in use and marking them. Any objects that are not marked are considered to be eligible for garbage collection and their memory is freed up.

- Generational: This algorithm is based on the observation that most objects have a short lifetime and are only used for a brief period of time. The managed heap is divided into three generations, with each generation representing objects that have been alive for a certain period of time. The GC uses different algorithms to collect objects in each generation. New objects are allocated in Generation 0, and if they survive a garbage collection, they are promoted to Generation 1. If they survive another garbage collection, they are promoted to Generation 2.

- Concurrent: This algorithm involves scanning the managed heap in parallel with the application's execution to identify objects that are no longer being used. This can help to minimize the impact of garbage collection on application performance.

The GC can be configured to use different algorithms and settings based on the needs of the application. For example, the GC can be configured to use more aggressive garbage collection to minimize memory usage or to use a more conservative approach to minimize the impact on application performance.

## Examples

Here are some basic examples of how the Garbage Collector works in C#:

1 - Object creation and destruction:

```C#
// Object creation
MyClass obj = new MyClass();

// Object destruction
obj = null;
```

In this example, a new instance of MyClass is created and assigned to the variable obj. Once obj is set to null, the MyClass instance becomes eligible for garbage collection.

2 - Forced garbage collection:

```C#
// Force garbage collection
GC.Collect();
```

In this example, the GC is forced to perform garbage collection. This can be useful in situations where memory usage needs to be optimized or when objects with finalizers need to be cleaned up.

3 - Using IDisposable interface:

```C#
// Using IDisposable interface
using (MyResource resource = new MyResource())
{
    // Use the resource here
}

// The resource is automatically disposed of once the using block is exited
```

In this example, the MyResource class implements the IDisposable interface, which allows the class to clean up any unmanaged resources it is using. By wrapping the instantiation of the MyResource class in a using block, the resource is automatically disposed of once the block is exited. This can help to ensure that resources are cleaned up in a timely and efficient manner.