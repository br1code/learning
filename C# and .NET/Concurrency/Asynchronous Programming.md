# Asynchronous Programming

TODO: 
- Watch https://youtu.be/3GhKdDCvtKE
- Watch https://youtu.be/il9gl8MH17s

## Simple explanation

Asynchronous programming allows you to execute long-running operations without blocking the main thread, which can improve the responsiveness of your application. Instead of waiting for an operation to complete, your code can continue executing and handle the result later when it's available.

## Deep explanation

Asynchronous programming in C# and .NET involves using asynchronous methods, which are methods that return a task that represents the asynchronous operation. When you call an asynchronous method, it starts executing the operation on a separate thread, and then returns the task immediately without blocking the calling thread. The task allows you to monitor the progress of the operation and retrieve the result when it's available.

## Examples
There are two main ways to use asynchronous programming in C# and .NET:

1 - Asynchronous methods: You can define your own asynchronous methods using the async and await keywords. These keywords allow you to write asynchronous code that looks similar to synchronous code, making it easier to read and write. Here's an example:

```C#
async Task<int> GetResultAsync()
{
    // Execute a long-running operation asynchronously
    int result = await LongRunningOperationAsync();
    
    // Return the result
    return result;
}
```

2 - Asynchronous APIs: Many APIs in .NET provide asynchronous versions of their methods that you can use to perform long-running operations. These methods usually have the suffix "Async" and return a task that represents the operation. Here's an example:

```C#
public async Task DownloadFileAsync(string url, string fileName)
{
    using (var httpClient = new HttpClient())
    {
        var response = await httpClient.GetAsync(url);
        using (var stream = await response.Content.ReadAsStreamAsync())
        {
            using (var fileStream = new FileStream(fileName, FileMode.Create))
            {
                await stream.CopyToAsync(fileStream);
            }
        }
    }
}
```

## Tips

- Asynchronous programming is not always the best solution: While asynchronous programming can improve the performance and responsiveness of your application, it is not always the best solution. Asynchronous programming introduces some additional complexity, and in some cases, synchronous code might be simpler and easier to understand.

- Be aware of the context when working with asynchronous code: In some cases, asynchronous code can execute on different threads, which can cause issues with shared resources or with user interfaces. To avoid these issues, make sure to understand the context in which your asynchronous code executes and use appropriate techniques to synchronize access to shared resources or update user interfaces.

- Use the right tool for the job: C# and .NET offer several different techniques for asynchronous programming, including Tasks, async/await, and event-based programming. Each technique has its strengths and weaknesses, and you should choose the one that best fits your needs.

- Use cancellation tokens to gracefully cancel asynchronous operations: Asynchronous operations can take longer than expected, and it's important to provide a way for users to cancel them if needed. You can use cancellation tokens to allow users to cancel an asynchronous operation gracefully.

- Use asynchronous programming to improve scalability: Asynchronous programming can improve the scalability of your application by allowing it to handle more requests with fewer resources. By freeing up threads to handle other requests while waiting for I/O operations to complete, your application can handle more requests without the need for additional hardware.

- Be aware of the performance overhead of asynchronous programming: While asynchronous programming can improve the performance of your application in some cases, it also introduces some overhead. In some cases, the overhead of using asynchronous code might outweigh the benefits, so make sure to measure the performance of your code and compare it with synchronous code.

- In general, it's not recommended to use async-await in constructors in C#. This is because constructors cannot be awaited, and there is no way to block a constructor until asynchronous operations are completed. If you need to perform asynchronous operations during object initialization, you can consider using a factory method or a static factory method to create the object, which can then asynchronously perform the required operations before returning the instance. However, in some cases, it may be necessary to use async-await in a constructor, such as when initializing an object requires async I/O operations, and the caller needs to use the resulting object immediately after construction. In such cases, you can use a workaround such as creating an asynchronous factory method that internally uses a static constructor, which can perform the asynchronous initialization operations.

### Synchronization context

The synchronization context in C# and .NET is an object that manages the execution of code on a particular thread. Whenever a piece of code needs to execute on a specific thread, it can use the synchronization context to ensure that it runs on that thread.

In a typical desktop or server application, the synchronization context is set up automatically by the runtime environment, and most developers don't need to think about it. However, in certain situations, such as when working with asynchronous code or UI programming, understanding the synchronization context can be important.

For example, when using asynchronous programming with the async/await keywords, the synchronization context determines which thread the await keyword returns to after an asynchronous operation completes. In a UI application, this ensures that the code that updates the UI runs on the UI thread, preventing threading issues such as cross-thread access violations.

Overall, the synchronization context provides a way for code to execute on a specific thread and helps manage threading issues in concurrent and asynchronous programming.

### ConfigureAwait method

ConfigureAwait is a method used in asynchronous programming in C# that is used to control how a method continues execution after an await keyword. It specifies whether the method should continue execution on the current synchronization context or on a new context.

Here is the syntax of the ConfigureAwait method:

```C#
public ConfiguredTaskAwaitable ConfigureAwait(bool continueOnCapturedContext);
```
The ConfigureAwait method returns an object of type ConfiguredTaskAwaitable. This object is then used to either wait for the completion of a task, or to configure how the continuation of the task should be scheduled.

The continueOnCapturedContext parameter of the ConfigureAwait method specifies whether the method should continue execution on the current synchronization context (true) or on a new context (false). If true, the continuation will be scheduled to run on the same context as the current one. If false, the continuation will be scheduled to run on a new context.

By default, when an await keyword is used in a method, the method continues execution on the same synchronization context that it was started on. This means that if the method was started on a UI thread, for example, the continuation of the task will also run on the UI thread. This can be problematic, especially if the task is long-running, as it can cause the UI to become unresponsive.

Here is an example of how to use ConfigureAwait to specify that the continuation should **NOT** run on the current synchronization context:

```C#
public async Task DoSomethingAsync()
{
    var result = await SomeLongRunningOperation().ConfigureAwait(false);
    // continuation code here
}
```
In the example above, the continuation of the SomeLongRunningOperation task will be scheduled to run on a new context, which means that it will not block the UI thread.

It's important to note that ConfigureAwait should not be used in all cases. In fact, in most cases, it's better to let the default behavior of await handle the synchronization context. However, in certain situations, such as long-running operations or in performance-sensitive scenarios, using ConfigureAwait(false) can provide significant benefits.

In general, using ConfigureAwait(false) can improve performance by avoiding unnecessary thread context switches. This is especially true in web APIs, where the synchronization context is not necessary for most operations and can actually cause performance issues.

However, if you need to execute some code in a specific synchronization context, such as the UI thread in a GUI application, you should avoid using ConfigureAwait(false) so that the code continues executing in the correct context.

In the context of a web API, where there is no synchronization context that needs to be maintained, using ConfigureAwait(false) is generally recommended for performance reasons. This is because it allows the continuation to be executed on any thread from the thread pool, rather than being marshalled back to the original context, which can add overhead.

However, when working with databases, it is important to ensure that your code still executes within a transaction context, which may be dependent on the specific database provider being used. Some database providers may require that transactions be executed on a specific thread or within a specific synchronization context, and in these cases using ConfigureAwait(false) may not be appropriate.

When using Entity Framework with a SQL database, it is generally safe to use ConfigureAwait(false) for database queries since they are typically executed on a separate thread pool and do not require synchronization with any particular context. However, you should always test and measure the performance impact of using ConfigureAwait(false) to ensure it provides the expected performance benefits in your specific scenario.

Additionally, if you are using async-compatible versions of Entity Framework's DbContext methods (such as ToListAsync()), these methods already use ConfigureAwait(false) internally, so you do not need to use it explicitly in your code.