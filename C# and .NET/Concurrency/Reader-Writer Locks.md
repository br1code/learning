# Reader-Writer Locks

## Simple explanation

A reader-writer lock is a synchronization primitive that allows multiple threads to read a shared resource simultaneously, but only one thread to write to the resource at a time. Reader-writer locks can be useful when multiple threads need to read data, but only one thread should be able to modify the data at any given time.

## Deep explanation

In a concurrent program, multiple threads may need to access and modify the same shared data. In some cases, many threads may only need to read the data, while only one thread needs to modify it. In such scenarios, using a simple lock to synchronize access to the shared data can lead to poor performance, as multiple readers can be blocked by a single writer.

Reader-writer locks provide a more fine-grained mechanism for synchronizing access to shared data. With a reader-writer lock, multiple threads can acquire a shared (read) lock, which allows them to read the shared data simultaneously. However, when a thread wants to modify the shared data, it must first acquire an exclusive (write) lock, which prevents other threads from acquiring any lock on the shared data until the write operation is complete.

In C# and .NET, reader-writer locks are implemented using the ReaderWriterLockSlim class. This class provides methods for acquiring and releasing shared and exclusive locks. The EnterReadLock method acquires a shared lock, while the EnterWriteLock method acquires an exclusive lock. Once a thread has acquired a lock, it can perform its read or write operation on the shared data.

## Examples

```C#
// Example 1: Using a reader-writer lock to protect a list of integers
List<int> numbers = new List<int>();
ReaderWriterLockSlim rwLock = new ReaderWriterLockSlim();

// In this method, we add a new number to the list. We first acquire an exclusive (write) lock
// to ensure that no other thread is modifying the list. Once we have the lock, we add the new
// number to the list and release the lock.
public void AddNumber(int number)
{
    rwLock.EnterWriteLock();
    try
    {
        numbers.Add(number);
    }
    finally
    {
        rwLock.ExitWriteLock();
    }
}

// In this method, we return a copy of the list. We first acquire a shared (read) lock to ensure
// that no thread is modifying the list. Once we have the lock, we create a copy of the list and
// release the lock.
public List<int> GetNumbers()
{
    rwLock.EnterReadLock();
    try
    {
        return new List<int>(numbers);
    }
    finally
    {
        rwLock.ExitReadLock();
    }
}
```