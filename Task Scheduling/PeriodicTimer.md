# Periodic Timer

`PeriodicTimer` is a new addition to .NET 6 that simplifies implementing periodic actions in a non-blocking, asynchronous way. It is designed to run a callback method periodically using a specified interval. The primary use cases for `PeriodicTimer` are to execute recurring tasks, such as monitoring, background data processing, or cleanup activities.

## Creating a `PeriodicTimer`:

To create a `PeriodicTimer`, you need to specify the time interval between invocations of the callback method. The interval is expressed as a `TimeSpan` value. The timer will start immediately after creation.

Example:

```csharp
using System.Threading;

var timer = new PeriodicTimer(TimeSpan.FromSeconds(30));
```

In this example, a new `PeriodicTimer` is created with an interval of 30 seconds.

## Using `PeriodicTimer` in an asynchronous loop:

To use the `PeriodicTimer`, create an asynchronous loop that awaits the `WaitForNextTickAsync` method. This method returns a `ValueTask` that completes when the next timer tick occurs.

Example:

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

public class MyPeriodicTask
{
    public async Task RunAsync(CancellationToken cancellationToken)
    {
        using var timer = new PeriodicTimer(TimeSpan.FromSeconds(30));

        while (!cancellationToken.IsCancellationRequested)
        {
            await timer.WaitForNextTickAsync(cancellationToken);

            // Perform your periodic task here, e.g., fetch data from an API, process it, and store it in a database
        }
    }
}
```

In this example, the `RunAsync` method creates a `PeriodicTimer` with a 30-second interval and an asynchronous loop that runs until the provided `CancellationToken` is canceled. The loop body performs the periodic task.

Note: We use `using` when creating a new `PeriodicTimer` because it needs to be disposed.

## Handling errors and exceptions:

When using a `PeriodicTimer`, it's essential to handle errors and exceptions in the asynchronous loop. This can be done by implementing a try-catch block within the loop.

Example:

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

public class MyPeriodicTask
{
    public async Task RunAsync(CancellationToken cancellationToken)
    {
        using var timer = new PeriodicTimer(TimeSpan.FromSeconds(30));

        while (!cancellationToken.IsCancellationRequested)
        {
            try
            {
                await timer.WaitForNextTickAsync(cancellationToken);

                // Perform your periodic task here, e.g., fetch data from an API, process it, and store it in a database
            }
            catch (OperationCanceledException) when (cancellationToken.IsCancellationRequested)
            {
                // Graceful cancellation
                break;
            }
            catch (Exception ex)
            {
                // Handle exceptions and log errors
                Console.WriteLine($"An error occurred: {ex.Message}");
            }
        }
    }
}
```

In this example, a try-catch block is added to handle exceptions, log errors, and support graceful cancellation.

`PeriodicTimer` offers a simple way to implement periodic tasks in .NET 6+, making it easy to create recurring tasks that run asynchronously without blocking the main application thread.

## Implementing a Periodic Task

Here's an example of combining `PeriodicTimer` with a `BackgroundService` to implement a periodic task in a .NET 6+ application:

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;

public class PeriodicBackgroundService : BackgroundService
{
    private readonly ILogger<PeriodicBackgroundService> _logger;

    public PeriodicBackgroundService(ILogger<PeriodicBackgroundService> logger)
    {
        _logger = logger;
    }

    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        using var timer = new PeriodicTimer(TimeSpan.FromSeconds(30));

        while (!stoppingToken.IsCancellationRequested)
        {
            try
            {
                await timer.WaitForNextTickAsync(stoppingToken);

                // Perform your periodic task here, e.g., fetch data from an API, process it, and store it in a database
                _logger.LogInformation("Periodic task executed at {Time}", DateTimeOffset.UtcNow);
            }
            catch (OperationCanceledException) when (stoppingToken.IsCancellationRequested)
            {
                // Graceful cancellation
                break;
            }
            catch (Exception ex)
            {
                // Handle exceptions and log errors
                _logger.LogError(ex, "An error occurred in the periodic background service.");
            }
        }
    }
}
```

In this example, we create a `PeriodicBackgroundService` class that inherits from `BackgroundService`. The `ExecuteAsync` method creates a `PeriodicTimer` with a 30-second interval and an asynchronous loop that runs until the provided `stoppingToken` is canceled. The loop body performs the periodic task, logs its execution time, and handles exceptions and graceful cancellation.

The service is registered in `IHostBuilder.ConfigureServices` (`Program.cs`) with the `AddHostedService` extension method:

```csharp
services.AddHostedService<PeriodicBackgroundService>();
```

With this setup, the `PeriodicBackgroundService` will automatically start when the application starts and stop when the application shuts down. The periodic task will be executed every 30 seconds in a non-blocking, asynchronous manner.