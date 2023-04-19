# Locking

## Simple Explanation

Locking is a mechanism in C# and .NET that helps to synchronize access to shared resources, such as variables or objects, in a multithreaded environment. It ensures that only one thread at a time can access the shared resource, preventing conflicts and data corruption.

## Deep Explanation

In a multithreaded environment, where multiple threads are accessing the same shared resource, it is important to synchronize access to that resource to prevent conflicts and ensure data integrity. One way to achieve this is through locking. When a thread acquires a lock on a resource, no other thread can access that resource until the lock is released. This ensures that only one thread at a time can modify the resource, preventing data corruption and inconsistencies.

In C# and .NET, the lock statement is used to acquire and release locks. The syntax for the lock statement is:

```C#
lock (object)
{
    // Code to access shared resource
}
```

The object argument in the lock statement is a reference to the object that the lock will be applied to. This object serves as the synchronization object or mutex, and it is used to control access to the shared resource.

When a thread encounters a lock statement, it attempts to acquire the lock on the specified object. If the lock is already held by another thread, the current thread is blocked until the lock is released. Once the lock is acquired, the thread can safely access the shared resource. When the thread is done modifying the resource, it releases the lock by exiting the lock statement. This allows other threads to acquire the lock and access the shared resource.

## Examples

Here's an example of how to use locking to synchronize access to a shared resource in C#:

```C#
public class SharedCounter
{
    private int _count = 0;

    public void Increment()
    {
        lock (this)
        {
            _count++;
        }
    }

    public int Count
    {
        get
        {
            lock (this)
            {
                return _count;
            }
        }
    }
}
```