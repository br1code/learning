# RabbitMQ

## Introduction to RabbitMQ

- RabbitMQ is an open-source message broker that implements the Advanced Message Queuing Protocol (AMQP). It enables communication between components by routing messages through exchanges and queues. RabbitMQ provides robust messaging capabilities, including message persistence, clustering, and high availability.

- To install RabbitMQ, follow the instructions on the official website: https://www.rabbitmq.com/download.html. After installing, you can configure RabbitMQ using configuration files or environment variables. The RabbitMQ Management Plugin provides a web-based interface for managing and monitoring your RabbitMQ instance.

## Working with Queues

- Queues in RabbitMQ are the primary mechanism for storing and forwarding messages. Producers send messages to queues, while consumers retrieve and process messages from queues. Queues can be durable (persisted to disk) or transient (stored in memory).

- To create a queue using RabbitMQ, you can use the management plugin or a client library, like the RabbitMQ .NET client. In .NET, you can create a queue as follows:

```csharp
using RabbitMQ.Client;

var factory = new ConnectionFactory() { HostName = "localhost" };
using var connection = factory.CreateConnection();
using var channel = connection.CreateModel();
channel.QueueDeclare(queue: "myQueue", durable: false, exclusive: false, autoDelete: false, arguments: null);
```

- To consume messages from a queue using .NET code:

```csharp
using RabbitMQ.Client;
using RabbitMQ.Client.Events;
using System.Text;

var factory = new ConnectionFactory() { HostName = "localhost" };
using var connection = factory.CreateConnection();
using var channel = connection.CreateModel();
var consumer = new EventingBasicConsumer(channel);
consumer.Received += (model, ea) =>
{
   var body = ea.Body.ToArray();
   var message = Encoding.UTF8.GetString(body);
   Console.WriteLine("Received: {0}", message);
};
channel.BasicConsume(queue: "myQueue", autoAck: true, consumer: consumer);
```

- To publish messages to a queue using .NET code:

```csharp
using RabbitMQ.Client;
using System.Text;

var factory = new ConnectionFactory() { HostName = "localhost" };
using var connection = factory.CreateConnection();
using var channel = connection.CreateModel();
var body = Encoding.UTF8.GetBytes("Hello, RabbitMQ!");
channel.BasicPublish(exchange: "", routingKey: "myQueue", basicProperties: null, body: body);
```

## Exchanges and Bindings

- Exchanges in RabbitMQ are responsible for routing messages to queues based on routing keys, bindings, and exchange types (direct, fanout, topic, or headers). Bindings define the relationship between an exchange and a queue, determining how messages are routed.

- To create an exchange and bind it to a queue using RabbitMQ, you can use the management plugin or the .NET client library:

```csharp
using RabbitMQ.Client;

var factory = new ConnectionFactory() { HostName = "localhost" };
using var connection = factory.CreateConnection();
using var channel = connection.CreateModel();
channel.ExchangeDeclare(exchange: "myExchange", type: ExchangeType.Direct);
channel.QueueDeclare(queue: "myQueue", durable: false, exclusive: false, autoDelete: false, arguments: null);
channel.QueueBind(queue: "myQueue", exchange: "myExchange", routingKey: "myRoutingKey");
```

- To use .NET code to publish messages to an exchange and have them routed to a queue, you can modify the message publishing code we saw earlier:

```csharp
using RabbitMQ.Client;
using System.Text;

var factory = new ConnectionFactory() { HostName = "localhost" };
using var connection = factory.CreateConnection();
using var channel = connection.CreateModel();
var body = Encoding.UTF8.GetBytes("Hello, RabbitMQ!");
channel.BasicPublish(exchange: "myExchange", routingKey: "myRoutingKey", basicProperties: null, body: body);
```

In this example, we specified the "myExchange" exchange and "myRoutingKey" routing key when calling the `BasicPublish` method. RabbitMQ routes the message to the "myQueue" queue based on the binding we created earlier.

## Message Properties

- Message properties in RabbitMQ are metadata associated with messages. They can be used to control various aspects of message delivery, such as content encoding, content type, priority, or delivery mode (persistent or non-persistent). They can also include custom headers for application-specific purposes.

- To set and read message properties using .NET code, you can use the `IBasicProperties` interface from the RabbitMQ .NET client:

```csharp
// Setting message properties
var properties = channel.CreateBasicProperties();
properties.ContentType = "text/plain";
properties.DeliveryMode = 2; // Persistent
properties.Priority = 1;

// Publishing a message with properties
channel.BasicPublish(exchange: "myExchange", routingKey: "myRoutingKey", basicProperties: properties, body: body);

// Reading message properties in a consumer
consumer.Received += (model, ea) =>
{
   var receivedProperties = ea.BasicProperties;
   var contentType = receivedProperties.ContentType;
   var deliveryMode = receivedProperties.DeliveryMode;
   var priority = receivedProperties.Priority;
   // Process the message...
};
```

## Advanced RabbitMQ Concepts
- Message Acknowledgement and Delivery Guarantees: To ensure that messages are not lost, RabbitMQ supports message acknowledgements. Consumers can send an acknowledgement to RabbitMQ when they successfully process a message, allowing RabbitMQ to remove it from the queue. To enable this, set the `autoAck` parameter to `false` when calling `BasicConsume`. In .NET, use `channel.BasicAck(ea.DeliveryTag, false)` to acknowledge a message.

- Message Routing using Routing Keys and Topic Exchanges: Topic exchanges enable more complex routing based on patterns in routing keys. Producers send messages to a topic exchange with a routing key, and the exchange routes the message to queues bound with matching patterns. In .NET, declare a topic exchange with `channel.ExchangeDeclare("topicExchange", ExchangeType.Topic)` and bind a queue with a pattern using `channel.QueueBind("myQueue", "topicExchange", "pattern.*")`.

- Dead Letter Exchanges and Queues: Dead letter exchanges (DLX) handle messages that could not be processed, such as those that exceed a maximum number of retries or have expired. To use a DLX, create a new exchange and queue for dead-lettered messages, and set the `x-dead-letter-exchange` argument when declaring the original queue.

- Message Compression and Encryption: RabbitMQ messages can be compressed or encrypted for efficiency or security reasons. Compression can be achieved using standard .NET libraries like `System.IO.Compression`. Encryption can be implemented using .NET's cryptographic libraries like `System.Security.Cryptography`.

## Best Practices
- Error Handling and Retry Mechanisms: Implement error handling in consumers to handle exceptions and avoid losing messages. Use retry mechanisms, like exponential backoff, to re-process failed messages.

- Load Balancing and Scaling: Distribute load across multiple consumers or nodes by using multiple queues, exchanges, or RabbitMQ cluster features. Use horizontal scaling to increase the capacity of your RabbitMQ deployment.

- Monitoring and Management using RabbitMQ Management Plugin: Use the RabbitMQ Management Plugin to monitor the health and performance of your RabbitMQ instance. This tool provides a web-based interface for managing queues, exchanges, bindings, and more. Additionally, it offers monitoring capabilities, such as tracking message rates, queue lengths, and resource usage.

## Examples

- Program 1: A web API with just 1 endpoint that receives a text and sends it as a message to a RabbitMQ queue
- Program 2: A console program consuming messages from a RabbitMQ queue and writing each received message to a csv file

Program 1: Web API

1. First, create a new .NET Core Web API project using the following command:
```
dotnet new webapi -n RabbitMQWebAPI
```
2. Navigate to the project folder and install the RabbitMQ .NET client:
```
cd RabbitMQWebAPI
dotnet add package RabbitMQ.Client
```
3. Open the project in your favorite IDE and replace the contents of `Controllers/WeatherForecastController.cs` with the following code:

```csharp
using System;
using System.Text;
using Microsoft.AspNetCore.Mvc;
using RabbitMQ.Client;

namespace RabbitMQWebAPI.Controllers
{
    [ApiController]
    [Route("[controller]")]
    public class MessageController : ControllerBase
    {
        [HttpPost]
        public IActionResult Post([FromBody] string message)
        {
            var factory = new ConnectionFactory() { HostName = "localhost" };
            using var connection = factory.CreateConnection();
            using var channel = connection.CreateModel();
            channel.QueueDeclare(queue: "messages", durable: false, exclusive: false, autoDelete: false, arguments: null);

            var body = Encoding.UTF8.GetBytes(message);
            channel.BasicPublish(exchange: "", routingKey: "messages", basicProperties: null, body: body);

            return Ok("Message sent to RabbitMQ");
        }
    }
}
```

4. Run the Web API using the following command:
```
dotnet run
```

Program 2: Console Application

1. Create a new .NET Core Console Application using the following command:
```
dotnet new console -n RabbitMQConsumer
```
2. Navigate to the project folder and install the RabbitMQ .NET client:
```
cd RabbitMQConsumer
dotnet add package RabbitMQ.Client
```
3. Open the project in your favorite IDE and replace the contents of `Program.cs` with the following code:

```csharp
using System;
using System.IO;
using System.Text;
using RabbitMQ.Client;
using RabbitMQ.Client.Events;

namespace RabbitMQConsumer
{
    class Program
    {
        static void Main(string[] args)
        {
            var factory = new ConnectionFactory() { HostName = "localhost" };
            using var connection = factory.CreateConnection();
            using var channel = connection.CreateModel();
            channel.QueueDeclare(queue: "messages", durable: false, exclusive: false, autoDelete: false, arguments: null);

            var consumer = new EventingBasicConsumer(channel);
            consumer.Received += (model, ea) =>
            {
                var body = ea.Body.ToArray();
                var receivedMessage = Encoding.UTF8.GetString(body);
                Console.WriteLine("Received: {0}", receivedMessage);

                using var writer = new StreamWriter("output.csv", true);
                writer.WriteLine(receivedMessage);
            };
            channel.BasicConsume(queue: "messages", autoAck: true, consumer: consumer);

            Console.WriteLine("Press [enter] to exit.");
            Console.ReadLine();
        }
    }
}
```

4. Run the Console Application using the following command:
```
dotnet run
```

Now, when you send a POST request with a text message to the Web API (Program 1), it will publish the message to the RabbitMQ queue. The Console Application (Program 2) will consume the messages from the queue and write them to an `output.csv` file in the current directory.