# Polly

## Simple Explanation

Polly is a .NET library that allows you to handle different types of exceptions and retries in a simple and efficient way. With Polly, you can define policies that specify how your application should behave when something goes wrong, such as when an exception is thrown or when a specific HTTP response is returned. You can use Polly to retry failed operations, circuit breakers, and handle timeouts, among other things.

## Deep Explanation

Polly is a popular open-source library for .NET that provides a simple and elegant way to handle exceptions and retries. It's designed to help you write more robust and resilient applications that can handle different types of errors in a graceful and efficient way.

Polly uses the concept of policies to define how your application should behave when things go wrong. A policy is a set of rules that define how to handle a specific type of exception or a specific type of response from a service. For example, you can define a policy that retries an operation three times with an exponential backoff strategy if a 500 internal server error is returned.

Polly supports a wide range of policies, including:

- Retry policies: Retry an operation a specified number of times with a specified delay between retries.

- Circuit breaker policies: Open the circuit when a specified number of failures occur in a specified time period.

- Timeout policies: Cancel an operation if it takes too long to complete.

- Fallback policies: Provide a default value or a fallback operation if the main operation fails.

- Bulkhead isolation policies: Limit the number of concurrent requests that can be made to a particular resource.

Polly is flexible and can be used with any .NET application, including ASP.NET Core, WPF, Windows Forms, and more. It's also easy to use and can be integrated into your application with just a few lines of code.

## Examples

Here's a basic example that shows how to use Polly to retry an operation three times with an exponential backoff strategy:

```C#
var retryPolicy = Policy
    .Handle<HttpRequestException>()
    .WaitAndRetryAsync(3, retryAttempt => TimeSpan.FromSeconds(Math.Pow(2, retryAttempt)));

await retryPolicy.ExecuteAsync(async () =>
{
    using var httpClient = new HttpClient();
    var response = await httpClient.GetAsync("https://example.com/api/users");
    response.EnsureSuccessStatusCode();
    var content = await response.Content.ReadAsStringAsync();
    Console.WriteLine(content);
});
```

In this example, we define a retryPolicy that handles HttpRequestException exceptions and retries the operation three times with an exponential backoff strategy. We then use the policy to execute an HTTP GET request to https://example.com/api/users. If the request fails, Polly will automatically retry the operation according to the retry policy.

---

Here's another example that shows how to use Polly to handle circuit breaking:

```C#
var circuitBreakerPolicy = Policy
    .Handle<HttpRequestException>()
    .CircuitBreakerAsync(3, TimeSpan.FromSeconds(30));

await circuitBreakerPolicy.ExecuteAsync(async () =>
{
    using var httpClient = new HttpClient();
    var response = await httpClient.GetAsync("https://example.com/api/users");
    response.EnsureSuccessStatusCode();
    var content = await response.Content.ReadAsStringAsync();
    Console.WriteLine(content);
});
```

In this example, we define a circuitBreakerPolicy that handles HttpRequestException exceptions and breaks the circuit when three failures occur within a 30-second time window. If the circuit is open, Polly will automatically reject subsequent requests until the circuit is closed again.

---

Here's an example of configuring Polly in a ASP.NET Web API application:

```C#
using Microsoft.Extensions.DependencyInjection;
using Polly;
using Polly.Extensions.Http;
using System;

public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("myHttpClient")
            .AddPolicyHandler(GetRetryPolicy())
            .AddPolicyHandler(GetCircuitBreakerPolicy());

    services.AddSingleton<IRepository, MyRepository>();

    services.AddPolicyRegistry(GetDefaultPolicies());
}

private static IAsyncPolicy<HttpResponseMessage> GetRetryPolicy()
{
    return HttpPolicyExtensions
        .HandleTransientHttpError()
        .OrResult(msg => msg.StatusCode == System.Net.HttpStatusCode.NotFound)
        .WaitAndRetryAsync(3, retryAttempt => TimeSpan.FromSeconds(Math.Pow(2, retryAttempt)));
}

private static IAsyncPolicy<HttpResponseMessage> GetCircuitBreakerPolicy()
{
    return HttpPolicyExtensions
        .HandleTransientHttpError()
        .CircuitBreakerAsync(5, TimeSpan.FromSeconds(30));
}

private static IDictionary<string, IAsyncPolicy<HttpResponseMessage>> GetDefaultPolicies()
{
    return new Dictionary<string, IAsyncPolicy<HttpResponseMessage>>
    {
        { "retryPolicy", GetRetryPolicy() },
        { "circuitBreakerPolicy", GetCircuitBreakerPolicy() }
    };
}
```

Here, we've added the services.AddPolicyRegistry(GetDefaultPolicies()) call to register the policies with the policy registry. This allows us to later inject and use the policies in our services and controllers by their registered names, such as "retryPolicy" and "circuitBreakerPolicy".

To use the AsyncPolicy in an HTTP request, you can simply wrap your HTTP call inside a delegate, and then execute that delegate using the AsyncPolicy.ExecuteAsync method. Here's an example:

```C#
using System;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Polly;
using Polly.Registry;

namespace MyApi.Controllers
{
    [ApiController]
    [Route("[controller]")]
    public class MyController : ControllerBase
    {
        private readonly IHttpClientFactory _httpClientFactory;
        private readonly IPolicyRegistry<string> _policyRegistry;

        public MyController(IHttpClientFactory httpClientFactory, IPolicyRegistry<string> policyRegistry)
        {
            _httpClientFactory = httpClientFactory;
            _policyRegistry = policyRegistry;
        }

        [HttpGet]
        public async Task<IActionResult> Get()
        {
            var httpClient = _httpClientFactory.CreateClient();

            // Registered Policies: retryPolicy, circuitBreakerPolicy
            var policy = _policyRegistry.Get<AsyncPolicy>("MyPolicy");

            var result = await policy.ExecuteAsync(async () =>
            {
                var response = await httpClient.GetAsync("https://api.example.com/data");

                if (response.IsSuccessStatusCode)
                {
                    var content = await response.Content.ReadAsStringAsync();
                    return Ok(content);
                }

                return StatusCode((int)response.StatusCode);
            });

            return result;
        }
    }
}
```

---

You can combine multiple policies using the PolicyWrap class provided by Polly. The PolicyWrap class allows you to combine policies in a specific order to create a single policy.

Here's an example of how you can combine the retryPolicy and circuitBreakerPolicy defined earlier using a PolicyWrap:

```C#
var retryPolicy = Policy
    .Handle<HttpRequestException>()
    .WaitAndRetryAsync(3, retryAttempt => TimeSpan.FromSeconds(Math.Pow(2, retryAttempt)));

var circuitBreakerPolicy = Policy
    .Handle<HttpRequestException>()
    .CircuitBreakerAsync(
        exceptionsAllowedBeforeBreaking: 2,
        durationOfBreak: TimeSpan.FromSeconds(30));

var policyWrap = Policy.WrapAsync(retryPolicy, circuitBreakerPolicy);

var httpClient = new HttpClient();

var httpResponse = await policyWrap.ExecuteAsync(async () =>
{
    var httpRequest = new HttpRequestMessage(HttpMethod.Get, "https://example.com");
    return await httpClient.SendAsync(httpRequest);
});
```

In the example above, we first define the retryPolicy and circuitBreakerPolicy policies as before. Then, we create a PolicyWrap instance by calling the Policy.WrapAsync method and passing the policies in the order we want them to be executed. Finally, we execute the HTTP request by calling the ExecuteAsync method on the policyWrap instance.

When the HTTP request is executed, the policyWrap will first execute the retryPolicy, which will retry the request up to three times with an exponential backoff delay. If the retryPolicy fails after three attempts, the policyWrap will then execute the circuitBreakerPolicy, which will break the circuit for 30 seconds if there are two or more exceptions in a given time window.

## Notes

### Exponential Backoff Strategy

An exponential backoff strategy is a technique used in software development to manage retries in case of failures. It involves increasing the amount of time between retries in an exponential manner, to avoid overwhelming the system with too many retries too quickly.

The idea behind an exponential backoff strategy is that if a retry fails, the next retry should be delayed by a certain amount of time. If that retry also fails, the next retry should be delayed by a longer amount of time, and so on, until either the maximum number of retries has been reached or the operation succeeds.

The delay between retries increases exponentially, typically by multiplying the previous delay by a constant factor. This ensures that the delay grows quickly but not too fast. The goal is to balance the need to give the system time to recover with the need to retry the operation as soon as possible.

Exponential backoff is often used in distributed systems, where network failures, service outages, and other issues can cause failures. By using this strategy, the system can automatically recover from these issues without requiring manual intervention.

### Circuit Breaking

Circuit breaking is a design pattern used to improve the resilience and reliability of a software system. It is a mechanism that allows the system to detect when a remote service or resource is unavailable, and to stop sending requests to it for a period of time. This helps to prevent the system from becoming overwhelmed by requests, which could cause it to slow down or even crash.

The circuit breaker works by monitoring the response of the remote service or resource to which it is connected. If the response is slow or contains errors, the circuit breaker will trip and prevent any further requests from being sent. Once the circuit breaker is tripped, it will wait for a configurable amount of time before allowing requests to be sent again. This gives the remote service or resource time to recover and respond to requests more quickly.

In addition to providing a way to avoid overwhelming a remote service or resource, circuit breaking can also help to improve the overall resilience of a system. By isolating a failing service or resource, the circuit breaker prevents cascading failures from affecting other parts of the system.

