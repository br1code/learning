# Large Object Heap

## Simple Explanation

The Large Object Heap (LOH) is a section of the managed heap in the .NET runtime that is used to store large objects, which are those that require more than 85,000 bytes of memory. The LOH has some performance characteristics that are different from the rest of the heap, which can affect the performance of your application if you allocate and deallocate large objects frequently.

## Deep Explanation

In the .NET runtime, the heap is the area of memory where objects are allocated. The managed heap is divided into three parts: the ephemeral segment, the Large Object Heap, and the loader heap. The ephemeral segment is used to store short-lived objects, while the loader heap is used to store the metadata for loaded assemblies.

The Large Object Heap is used to store large objects, which are those that require more than 85,000 bytes of memory. Large objects are stored on the LOH to avoid fragmentation of the ephemeral segment, which could cause performance issues. When an object is allocated on the LOH, it is allocated in a contiguous block of memory, which can lead to fragmentation of the LOH itself.

The LOH has some performance characteristics that are different from the rest of the heap. For example, because the LOH is not compacted as often as the rest of the heap, it can become fragmented over time. This can lead to performance issues if you allocate and deallocate large objects frequently. In addition, because objects on the LOH are larger than those on the ephemeral segment, they take longer to allocate and deallocate.

## Examples

Here's an example of how to allocate a large object on the LOH in C#:

```C#
byte[] largeObject = new byte[100000];
```

In this example, a byte array with a length of 100,000 bytes is allocated on the LOH.

To check whether an object is stored on the LOH, you can use the GC.GetGeneration method, which returns the generation number of an object:

```C#
int generation = GC.GetGeneration(largeObject);
if (generation == GC.MaxGeneration)
{
    Console.WriteLine("The large object is stored on the LOH.");
}
```