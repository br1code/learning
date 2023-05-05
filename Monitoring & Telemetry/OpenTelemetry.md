# Open Telemetry

OpenTelemetry is a set of APIs, libraries, agents, and instrumentation resources designed to provide observability for applications. It is a project under the Cloud Native Computing Foundation (CNCF) that aims to provide a unified approach to collecting metrics, logs, and traces from distributed systems. OpenTelemetry offers a vendor-neutral and standardized way to collect telemetry data, which allows for easier integration with various monitoring and observability platforms.

Using OpenTelemetry for Monitoring and Telemetry:

1. Instrumentation: OpenTelemetry provides both automatic and manual instrumentation options. Automatic instrumentation involves using pre-built libraries or agents to instrument your application with minimal code changes, while manual instrumentation requires adding specific OpenTelemetry API calls within your application code.

2. Tracing: OpenTelemetry supports distributed tracing, which enables tracking individual requests or transactions as they traverse through multiple services in a distributed system. This helps identify bottlenecks and diagnose performance issues.

3. Metrics: OpenTelemetry enables the collection of metrics such as response times, error rates, and resource utilization. These metrics can be used to monitor system performance, set alerts, and guide optimization efforts.

4. Exporters: OpenTelemetry supports various exporters, which allow you to send collected telemetry data to different monitoring and observability platforms, such as Prometheus, Jaeger, or Azure Monitor.

## Example using .NET

Source: https://opentelemetry.io/docs/instrumentation/net/getting-started/

To get started with OpenTelemetry in a .NET application, follow these steps:

1. Install the OpenTelemetry packages:

Add the appropriate NuGet packages to your project. For example, to add tracing instrumentation and export traces to the console, you would install the following packages:

```
dotnet add package OpenTelemetry.Exporter.Console
dotnet add package OpenTelemetry.Extensions.Hosting
```

Now let’s add the instrumentation packages for ASP.NET Core. This will give us some automatic spans for each HTTP request to our app.

```
dotnet add package OpenTelemetry.Instrumentation.AspNetCore --prerelease
```

2. Configure OpenTelemetry:

In your `Program.cs`, configure OpenTelemetry using the `AddOpenTelemetry` method:

```csharp
using System.Diagnostics;
using OpenTelemetry.Resources;
using OpenTelemetry.Trace;

var builder = WebApplication.CreateBuilder(args);

// .. other setup

builder.Services.AddOpenTelemetry()
    .WithTracing(tracerProviderBuilder =>
        tracerProviderBuilder
            .AddSource(DiagnosticsConfig.ActivitySource.Name)
            .ConfigureResource(resource => resource
                .AddService(DiagnosticsConfig.ServiceName))
            .AddAspNetCoreInstrumentation()
            .AddConsoleExporter());

// ... other setup

public static class DiagnosticsConfig
{
    public const string ServiceName = "MyService";
    public static ActivitySource ActivitySource = new ActivitySource(ServiceName);
}
```

3. Run your application:

With the instrumentation in place, you can now run your .NET application. You should be able to run your site, and see a Console output similar to this:

```
Activity.TraceId:            54d084eba205a7a39398df4642be8f4a
Activity.SpanId:             aca5e39a86a17d59
Activity.TraceFlags:         Recorded
Activity.ActivitySourceName: Microsoft.AspNetCore
Activity.DisplayName:        /
Activity.Kind:               Server
Activity.StartTime:          2023-02-21T12:19:28.2499974Z
Activity.Duration:           00:00:00.3106744
Activity.Tags:
    net.host.name: localhost
    net.host.port: 5123
    http.method: GET
    http.scheme: http
    http.target: /
    http.url: http://localhost:5123/
    http.flavor: 1.1
    http.user_agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.0.0 Safari/537.36 Edg/110.0.1587.50
    http.status_code: 200
Resource associated with Activity:
    service.name: MyService
    service.instance.id: 2c7ca153-e460-4643-b550-7c08487a4c0c
```

To collect metrics and logs, you can follow similar steps, installing the relevant packages and configuring the corresponding exporters and instrumentation.

By using OpenTelemetry in your .NET application, you can gain valuable insights into the performance and behavior of your system, which can help with debugging, optimization, and ensuring a better user experience.

More Examples: https://github.com/open-telemetry/opentelemetry-dotnet/tree/main/examples

## Export to Jaeger

Jaeger is an open-source distributed tracing system developed by Uber Technologies and now part of the Cloud Native Computing Foundation (CNCF). It is designed to monitor and troubleshoot transactions in complex, distributed systems by providing end-to-end visibility into the flow of requests across services. Jaeger can help developers identify performance bottlenecks, diagnose issues, and understand the interactions between different services in a microservices architecture.

In the context of monitoring and telemetry, Jaeger serves as a backend for collecting, storing, and analyzing trace data. It works with various instrumentation libraries and frameworks, including OpenTelemetry, which can collect and export trace data from your applications to the Jaeger backend.

Here's an overview of Jaeger's architecture and components:

1. Jaeger Client: The Jaeger Client is a library that can be integrated into your application to generate and report trace data. It communicates with the Jaeger Agent to send trace data over the network.

2. Jaeger Agent: The Jaeger Agent is a lightweight, stateless daemon that runs on the same host as your application. It receives trace data from the Jaeger Client and forwards it to the Jaeger Collector over the network.

3. Jaeger Collector: The Jaeger Collector is a centralized component that receives trace data from multiple Jaeger Agents. It processes, validates, and stores the data in a backend storage system, such as Cassandra or Elasticsearch.

4. Jaeger Query: The Jaeger Query is a service that provides a RESTful API and a web-based user interface to search, visualize, and analyze trace data stored in the backend storage system.

5. Storage Backend: Jaeger supports various storage backends for persisting trace data, including Cassandra, Elasticsearch, and Kafka. The choice of storage backend depends on factors such as scalability, query performance, and data retention requirements.

In the context of using OpenTelemetry and Jaeger in a .NET application, Jaeger serves as the trace data storage and visualization platform. The OpenTelemetry instrumentation in your .NET application collects trace data and exports it to Jaeger using the Jaeger exporter. You can then use the Jaeger Query web UI or API to search, visualize, and analyze the trace data to gain insights into your application's performance and behavior.

By integrating Jaeger with your distributed system, you can better understand the complex interactions between services, identify performance bottlenecks, and diagnose issues more effectively.

Here's how to configure your application to export to Jaeger:

appsettings.json
```JSON
{
    "Jaeger": {
    "AgentHost": "localhost",
    "AgentPort": 6831,
    "Endpoint": "http://localhost:14268",
    "Protocol": "UdpCompactThrift"
    }
}
```

Program.cs
```csharp
// ...
builder.AddJaegerExporter();

builder.ConfigureServices(services =>
{
    // Use IConfiguration binding for Jaeger exporter options.
    services.Configure<JaegerExporterOptions>(appBuilder.Configuration.GetSection("Jaeger"));

    // Customize the HttpClient that will be used when JaegerExporter is configured for HTTP transport.
    services.AddHttpClient("JaegerExporter", configureClient: (client) => client.DefaultRequestHeaders.Add("X-MyCustomHeader", "value"));
});
```