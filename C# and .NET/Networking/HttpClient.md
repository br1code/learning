# HttpClient

## Explanation

HttpClient is a class in the System.Net.Http namespace that provides a simple and efficient way to make HTTP requests to web servers. It's a core component of the .NET framework and is widely used in web applications.

HttpClient supports many features, such as sending and receiving HTTP headers, handling cookies, handling redirects, and more. It can also be used asynchronously, which is particularly useful when making multiple HTTP requests at once.

To use HttpClient, you first need to create an instance of it. You can then use the instance to send HTTP requests by creating HttpRequestMessage objects and passing them to the HttpClient's SendAsync method. The SendAsync method returns a Task<HttpResponseMessage> that you can use to receive the HTTP response.

## Examples

Here's an example of using HttpClient to send a GET request to a web server:

```C#
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static async Task Main(string[] args)
    {
        using var client = new HttpClient();
        var response = await client.GetAsync("https://example.com");
        var content = await response.Content.ReadAsStringAsync();
        Console.WriteLine(content);
    }
}

```

Here's an example of using HttpClient to send a POST request with JSON data to a web server:

```C#
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Text.Json;
using System.Threading.Tasks;

class Program
{
    static async Task Main(string[] args)
    {
        using var client = new HttpClient();
        var request = new HttpRequestMessage
        {
            Method = HttpMethod.Post,
            RequestUri = new Uri("https://example.com/api/users"),
            Content = new StringContent(JsonSerializer.Serialize(new { Name = "John", Age = 30 }), Encoding.UTF8, "application/json")
        };
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", "your-access-token");
        var response = await client.SendAsync(request);
        var content = await response.Content.ReadAsStringAsync();
        Console.WriteLine(content);
    }
}
```

## Tips

### Most recommended and correct way of using HttpClient

The most recommended and correct way of using HttpClient in modern .NET is to **use the HttpClientFactory** provided by the Microsoft.Extensions.Http NuGet package.

The HttpClientFactory manages the lifetime and configuration of HttpClient instances, and simplifies the process of creating and disposing of HttpClient objects. It also provides a way to apply global and per-client message handlers, which can add or modify HTTP headers, apply retry logic, and more.

To use HttpClientFactory, you need to register it with the dependency injection container in your application's startup code:

```C#
services.AddHttpClient();
```

This registers the HttpClientFactory with the default configuration, which includes a default HttpClient instance. You can then use this HttpClient instance by injecting IHttpClientFactory into your classes:

```C#
public class MyService
{
    private readonly IHttpClientFactory _httpClientFactory;

    public MyService(IHttpClientFactory httpClientFactory)
    {
        _httpClientFactory = httpClientFactory;
    }

    public async Task<string> GetGoogleHomePage()
    {
        var httpClient = _httpClientFactory.CreateClient();
        var response = await httpClient.GetAsync("https://www.google.com");
        return await response.Content.ReadAsStringAsync();
    }
}
```

This example shows how to use HttpClientFactory to create and use an HttpClient instance in a service. Note that the CreateClient method returns a new HttpClient instance that is configured with the default settings.

You can also configure named HttpClient instances with specific settings by using the AddHttpClient overload that takes a name parameter:

```C#
services.AddHttpClient("MyClient", httpClient =>
{
    httpClient.BaseAddress = new Uri("https://example.com");
    httpClient.DefaultRequestHeaders.Add("User-Agent", "My User Agent");
});
```

This registers a named HttpClient instance with a base address of https://example.com and a default user agent header. You can then use this named HttpClient instance by specifying the name in the CreateClient method:

```C#
var httpClient = _httpClientFactory.CreateClient("MyClient");
```