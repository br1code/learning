# Proxy

![](../Assets/proxy.png)

## Simple Explanation

The Proxy design pattern is a structural pattern that involves a class representing the functionality of another class. It acts as an interface or a surrogate to control access to the original object, allowing you to perform operations before or after forwarding the request to the actual object.

## Deep Explanation

The Proxy pattern has several use cases:

1. Remote Proxy: Represents an object in a different address space, allowing you to interact with a remote object as if it were local.

2. Virtual Proxy: Manages the creation of expensive resources and only creates the actual object when needed.

2. Protection Proxy: Controls access to the original object based on access rights, security levels, or other constraints.

A Proxy typically shares the same interface as the actual object (also called the Subject), making it easy to use in place of the Subject. Clients interact with the Proxy, which then handles the request, performs any necessary operations, and forwards the request to the actual object.

## Examples

Let's implement a protection proxy for a file access system:

1. Define the common interface for the Subject and the Proxy:

```C#
public interface IFileAccess
{
    void ReadFile(string fileName);
    void WriteFile(string fileName);
}
```

2. Implement the actual file access system (the Subject):

```C#
public class FileAccess : IFileAccess
{
    public void ReadFile(string fileName)
    {
        Console.WriteLine($"Reading file: {fileName}");
    }

    public void WriteFile(string fileName)
    {
        Console.WriteLine($"Writing file: {fileName}");
    }
}
```

3. Implement the Protection Proxy:

```C#
public class FileAccessProxy : IFileAccess
{
    private readonly FileAccess _fileAccess;
    private readonly string _userRole;

    public FileAccessProxy(string userRole)
    {
        _fileAccess = new FileAccess();
        _userRole = userRole;
    }

    public void ReadFile(string fileName)
    {
        Console.WriteLine("Checking access rights...");
        if (_userRole == "admin" || _userRole == "user")
        {
            _fileAccess.ReadFile(fileName);
        }
        else
        {
            Console.WriteLine("Access denied.");
        }
    }

    public void WriteFile(string fileName)
    {
        Console.WriteLine("Checking access rights...");
        if (_userRole == "admin")
        {
            _fileAccess.WriteFile(fileName);
        }
        else
        {
            Console.WriteLine("Access denied.");
        }
    }
}
```

4. Use the Protection Proxy:

```C#
class Program
{
    static void Main(string[] args)
    {
        var adminFileAccess = new FileAccessProxy("admin");
        adminFileAccess.ReadFile("example.txt");
        adminFileAccess.WriteFile("example.txt");

        var userFileAccess = new FileAccessProxy("user");
        userFileAccess.ReadFile("example.txt");
        userFileAccess.WriteFile("example.txt");

        var guestFileAccess = new FileAccessProxy("guest");
        guestFileAccess.ReadFile("example.txt");
        guestFileAccess.WriteFile("example.txt");
    }
}
```

In this example, `IFileAccess` is the common interface for the Subject (`FileAccess`) and the Proxy (`FileAccessProxy`). The `FileAccessProxy` controls access to the `FileAccess` object based on the user's role, allowing only users with appropriate permissions to read or write files. Clients interact with the `FileAccessProxy`, which then determines whether to forward the request to the `FileAccess` object or deny access.