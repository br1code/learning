# Azure Service Bus

## Introduction to Azure Service Bus

- Azure Service Bus is a fully managed, multi-protocol messaging service from Microsoft Azure. It enables communication between decoupled systems and components using messages. Azure Service Bus supports a variety of messaging patterns, including publish-subscribe, request-reply, and competing consumers. It provides advanced features like at-least-once delivery, message deduplication, and dead-lettering.

- Azure Service Bus offers multiple tiers, including Basic, Standard, and Premium, with different features and pricing options. Basic tier offers simple queue-based messaging, while Standard and Premium tiers provide more advanced features like topics, subscriptions, and duplicate detection. Pricing details can be found on the Azure Service Bus pricing page.

- To create an Azure Service Bus namespace, sign in to the Azure Portal and search for "Service Bus." Click "Create" and provide the required details, like subscription, resource group, namespace, and location. After creating the namespace, you can configure access policies and obtain connection strings from the "Shared access policies" section under the namespace settings.

## Working with Queues and Topics

- Queues and Topics are two key messaging entities in Azure Service Bus. Queues provide a simple first-in, first-out (FIFO) messaging pattern for point-to-point communication between a sender and a receiver. Topics enable a publish-subscribe pattern, where multiple subscribers can receive messages from a single publisher. Each subscriber receives a copy of the message by creating a Subscription.

- To create a Queue or Topic using the Azure Portal, navigate to your Service Bus namespace and click "Queues" or "Topics" from the left menu. Click "Add" and provide a name and other configuration options. To create a Queue or Topic using .NET code, you'll need the `Microsoft.Azure.ServiceBus` NuGet package. You can create a queue using the `ManagementClient` class as follows:

```csharp
var managementClient = new ManagementClient(connectionString);
await managementClient.CreateQueueAsync(new QueueDescription(queueName));
```

- To send and receive messages from a Queue or Topic, you can use the `QueueClient` or `TopicClient` classes from the `Microsoft.Azure.ServiceBus` package. The following example demonstrates sending and receiving messages from a queue:

```csharp
// Sending a message to a queue
var queueClient = new QueueClient(connectionString, queueName);
var message = new Message(Encoding.UTF8.GetBytes("Hello, Service Bus!"));
await queueClient.SendAsync(message);

// Receiving a message from a queue
queueClient.RegisterMessageHandler(async (receivedMessage, cancellationToken) =>
{
    var messageText = Encoding.UTF8.GetString(receivedMessage.Body);
    Console.WriteLine($"Received message: {messageText}");
    await queueClient.CompleteAsync(receivedMessage.SystemProperties.LockToken);
}, new MessageHandlerOptions(exceptionArgs => Task.CompletedTask) { AutoComplete = false });
```

- To create a Subscription for a Topic and receive messages, you'll need to use the `SubscriptionClient` class. Here's an example:

```csharp
// Create a subscription for a topic
var managementClient = new ManagementClient(connectionString);
await managementClient.CreateSubscriptionAsync(new SubscriptionDescription(topicName, subscriptionName));

// Receive messages from the subscription
var subscriptionClient = new SubscriptionClient(connectionString, topicName, subscriptionName);
subscriptionClient.RegisterMessageHandler(async (receivedMessage, cancellationToken) =>
{
    var messageText = Encoding.UTF8.GetString(receivedMessage.Body);
    Console.WriteLine($"Received message: {messageText}");
    await subscriptionClient.CompleteAsync(receivedMessage.SystemProperties.LockToken);
}, new MessageHandlerOptions(exceptionArgs => Task.CompletedTask) { AutoComplete = false });
```

Continuing from the previous example, here's how to send messages to a Topic:

```csharp
// Sending a message to a topic
var topicClient = new TopicClient(connectionString, topicName);
var message = new Message(Encoding.UTF8.GetBytes("Hello, Service Bus Topic!"));
await topicClient.SendAsync(message);
```

## Advanced Azure Service Bus Concepts

- Message Properties and Brokered Messages: In Azure Service Bus, messages are instances of the `Message` class, also known as brokered messages. These messages have a set of standard properties, like `MessageId`, `Label`, `ContentType`, `TimeToLive`, and `CorrelationId`, which can be used for message routing, filtering, and deduplication. Additionally, you can add custom properties using the `UserProperties` dictionary.

- Dead-Letter Queues and Poison Messages: A dead-letter queue (DLQ) is a separate queue that holds messages that cannot be processed or delivered for various reasons, such as exceeding the maximum delivery count or exceeding the time-to-live (TTL). These messages are called poison messages. Each queue and subscription in Azure Service Bus automatically has an associated DLQ. You can receive and process messages from the DLQ using a `QueueClient` or `SubscriptionClient` by specifying the dead-letter path.

- Scheduled Messages and Deferred Messages: Azure Service Bus allows you to schedule messages to be delivered at a specific time in the future. You can set the `ScheduledEnqueueTimeUtc` property on the `Message` object before sending it. Deferred messages are messages that are received but not processed immediately. Instead, they are set aside for later processing. To defer a message, call the `DeferAsync` method on the `QueueClient` or `SubscriptionClient`. To process a deferred message, use the `ReceiveDeferredMessageAsync` method with the `SequenceNumber` of the deferred message.

- Transactions and Sessions: Azure Service Bus supports transactional processing of messages, ensuring that a group of operations is either all successful or all fail. To use transactions, enable the `RequiresSession` property on a queue or topic. Sessions allow you to group related messages and process them in order, while also providing transactional guarantees. To send messages within a session, set the `SessionId` property on the `Message` object. To receive messages within a session, use the `SessionClient` class and call the `AcceptMessageSessionAsync` method.

## Azure Service Bus Best Practices

- Error Handling and Retry Mechanisms: Implement error handling and retry mechanisms to ensure resiliency in your messaging solution. The `Microsoft.Azure.ServiceBus` library provides a built-in `RetryPolicy` that you can configure with different retry strategies, such as exponential backoff. You can also implement your custom retry logic to handle specific exceptions.

- Load Balancing and Scaling: Azure Service Bus automatically manages load balancing and scaling. However, you can improve performance by using partitioned entities, which distribute the load across multiple message brokers. Also, consider using multiple namespaces for high-volume scenarios, and use competing consumers to increase the processing throughput.

- Monitoring and Management: Monitor your Azure Service Bus resources using the Azure Portal and Azure Monitor. The Azure Portal provides a dashboard with key metrics, such as active messages, dead-lettered messages, and message throughput. Azure Monitor allows you to collect and analyze detailed metrics and logs to identify issues, set up alerts, and create custom dashboards. Additionally, you can use the Azure Service Bus Management Library for .NET to programmatically manage and monitor your messaging entities.

## Examples

1. Program 1 - A web API that sends a message to an Azure Service Bus topic:

Create a new .NET Core Web API project and install the `Microsoft.Azure.ServiceBus` NuGet package. In your `appsettings.json`, add the following configuration:

```json
{
  "ServiceBus": {
    "ConnectionString": "<your-service-bus-connection-string>",
    "TopicName": "<your-topic-name>"
  }
}
```

Create a `ServiceBusController` with the following code:

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.ServiceBus;
using Microsoft.Extensions.Configuration;
using System.Text;
using System.Threading.Tasks;

namespace ServiceBusWebApi.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class ServiceBusController : ControllerBase
    {
        private readonly IConfiguration _configuration;

        public ServiceBusController(IConfiguration configuration)
        {
            _configuration = configuration;
        }

        [HttpPost]
        public async Task<IActionResult> SendMessage([FromBody] string messageText)
        {
            var connectionString = _configuration.GetValue<string>("ServiceBus:ConnectionString");
            var topicName = _configuration.GetValue<string>("ServiceBus:TopicName");
            var topicClient = new TopicClient(connectionString, topicName);

            var message = new Message(Encoding.UTF8.GetBytes(messageText));
            await topicClient.SendAsync(message);

            return Ok("Message sent to the Azure Service Bus topic.");
        }
    }
}
```

2. Program 2 - A console program that consumes messages from an Azure Service Bus topic and writes them to a CSV file:

Create a new .NET Core Console Application and install the `Microsoft.Azure.ServiceBus` NuGet package. In your `appsettings.json`, add the following configuration:

```json
{
  "ServiceBus": {
    "ConnectionString": "<your-service-bus-connection-string>",
    "TopicName": "<your-topic-name>",
    "SubscriptionName": "<your-subscription-name>"
  }
}
```

Replace the `Program.cs` file with the following code:

```csharp
using Microsoft.Azure.ServiceBus;
using Microsoft.Extensions.Configuration;
using System;
using System.IO;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace ServiceBusConsoleApp
{
    class Program
    {
        private static IConfiguration _configuration;
        private static SubscriptionClient _subscriptionClient;
        private static string _csvFilePath = "messages.csv";

        static async Task Main(string[] args)
        {
            _configuration = new ConfigurationBuilder()
                .AddJsonFile("appsettings.json", optional: false, reloadOnChange: true)
                .Build();

            var connectionString = _configuration.GetValue<string>("ServiceBus:ConnectionString");
            var topicName = _configuration.GetValue<string>("ServiceBus:TopicName");
            var subscriptionName = _configuration.GetValue<string>("ServiceBus:SubscriptionName");

            _subscriptionClient = new SubscriptionClient(connectionString, topicName, subscriptionName);

            var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
            {
                AutoComplete = false,
                MaxConcurrentCalls = 1
            };

            _subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);

            Console.WriteLine("Press ENTER to exit.");
            Console.ReadLine();

            await _subscriptionClient.CloseAsync();
        }

        private static async Task ProcessMessagesAsync(Message message, CancellationToken token)
        {
            var messageText = Encoding.UTF8.GetString(message.Body);
            Console.WriteLine($"Received message: {messageText}");

            using (var writer = new StreamWriter(_csvFilePath, true))
            {
                await writer.WriteLineAsync($"{DateTime.UtcNow},{messageText}");
            }

            await _subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
        }

        private static Task ExceptionReceivedHandler(ExceptionReceivedEventArgs exceptionReceivedEventArgs)
        {
            Console.WriteLine($"Message handler encountered an exception {exceptionReceivedEventArgs.Exception}.");
            var context = exceptionReceivedEventArgs.ExceptionReceivedContext;
            Console.WriteLine("Exception context for troubleshooting:");
            Console.WriteLine($"- Endpoint: {context.Endpoint}");
            Console.WriteLine($"- Entity Path: {context.EntityPath}");
            Console.WriteLine($"- Executing Action: {context.Action}");
            return Task.CompletedTask;
        }
    }
}
```

This console application will run continuously, processing messages from the Azure Service Bus topic subscription and writing them to a CSV file. When you're ready to stop the application, press ENTER.

Make sure to replace `<your-service-bus-connection-string>`, `<your-topic-name>`, and `<your-subscription-name>` with your actual Azure Service Bus connection string, topic name, and subscription name, respectively.

With these two programs, you can send messages from the web API to an Azure Service Bus topic and have the console program listen for messages and write them to a CSV file.

---

In summary, Azure Service Bus provides a robust messaging solution that enables decoupled communication between components in your software architecture. Queues and Topics are the primary entities that facilitate point-to-point and publish-subscribe messaging patterns, respectively. Using the .NET SDK, you can easily create, send, and receive messages from Queues, Topics, and Subscriptions, allowing you to build scalable and reliable messaging systems.