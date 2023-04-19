# Race conditions

## Simple Explanation

A race condition is a problem that can occur when two or more threads access a shared resource or variable simultaneously. The result of the operation can depend on the order in which the threads execute, leading to unpredictable or incorrect behavior.

## Deep Explanation

In a multi-threaded environment, race conditions can occur when multiple threads access a shared resource or variable, such as a shared memory location or a file. Race conditions can occur when two or more threads try to access the same resource at the same time, and the result of the operation depends on the order in which the threads execute.

For example, consider a scenario where two threads are trying to increment the value of a shared variable by 1. If both threads read the current value of the variable, increment it, and then write the new value back to the variable, a race condition can occur. If both threads read the current value of the variable at the same time, and then increment it and write it back at the same time, the value of the variable will only be incremented once, rather than twice, because the two increments effectively cancel each other out.

Race conditions can lead to unpredictable or incorrect behavior in a program. In order to avoid race conditions, it's important to use synchronization mechanisms such as locks, semaphores, or mutexes to ensure that only one thread can access a shared resource at a time.

## Examples

```C#
using System;
using System.Threading;

class Program
{
    static int sharedValue = 0;

    static void Main(string[] args)
    {
        Thread thread1 = new Thread(IncrementValue);
        Thread thread2 = new Thread(IncrementValue);

        thread1.Start();
        thread2.Start();

        thread1.Join();
        thread2.Join();

        Console.WriteLine("The value of sharedValue is: " + sharedValue);
    }

    static void IncrementValue()
    {
        for (int i = 0; i < 100000; i++)
        {
            int temp = sharedValue;
            temp++;
            sharedValue = temp;
        }
    }
}
```

In this example, two threads are created and both call the IncrementValue method. This method reads the current value of sharedValue, increments it, and writes it back to the variable. However, since two threads are accessing the same variable simultaneously, a race condition can occur. When this program is run, the final value of sharedValue can be unpredictable and different every time.