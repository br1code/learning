# Dapper

Dapper is a lightweight, high-performance Object-Relational Mapper (ORM) for .NET, created by the Stack Overflow team. It's designed to be easy-to-use, fast, and efficient, providing a simple way to map database query results to C# objects. Dapper is often chosen when performance is a priority, as it provides a more direct, low-level approach to data access compared to full-fledged ORMs like Entity Framework Core (EF Core).

## Key concepts

1. Simplicity: Dapper focuses on simplicity and ease-of-use, providing a minimalistic API for executing SQL queries and mapping the results to C# objects.

2. Performance: Dapper is designed for high performance and is considered one of the fastest ORMs for .NET, with minimal overhead and optimal query execution.

3. Flexibility: Dapper provides a high degree of control over the executed SQL queries, allowing you to optimize them for specific use cases and database systems.

4. No change tracking: Unlike EF Core, Dapper doesn't provide built-in change tracking, which means you'll need to handle updates and deletions manually.

## Examples

1. Querying data and mapping it to a C# object:

```csharp
public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
}

using (var connection = new SqlConnection("your_connection_string"))
{
    var posts = connection.Query<Post>("SELECT * FROM Posts").ToList();
}
```

2. Inserting data using a parameterized query:

```csharp
var newPost = new Post { Title = "New Post", Content = "Hello, World!" };

using (var connection = new SqlConnection("your_connection_string"))
{
    var affectedRows = connection.Execute("INSERT INTO Posts (Title, Content) VALUES (@Title, @Content)", newPost);
}
```

## Comparison between Dapper and Entity Framework Core

1. Performance: Dapper is generally faster than EF Core, as it provides a more direct, low-level approach to data access, with minimal overhead. EF Core is a full-fledged ORM with features like change tracking, identity management, and migrations, which can add some overhead.

2. Features: EF Core offers a comprehensive feature set, including change tracking, identity management, migrations, and support for LINQ queries. Dapper is more focused on simplicity and performance, providing a minimalistic API for executing SQL queries and mapping results to C# objects, without built-in support for advanced ORM features.

3. Complexity: Dapper is easy to learn and use, with a straightforward API that closely resembles ADO.NET. EF Core has a steeper learning curve and a more complex API, but it also provides more powerful features and abstractions that can simplify data access tasks.

4. Control: Dapper provides more control over the executed SQL queries, allowing you to optimize them for specific use cases and database systems. EF Core abstracts away many low-level details, which can make it more challenging to fine-tune queries for optimal performance.

5. Query generation: EF Core generates SQL queries from LINQ expressions, allowing you to write type-safe, expressive queries in C#. Dapper requires you to write raw SQL queries, which can be more error-prone and less maintainable.

---

In summary, Dapper is a lightweight, high-performance ORM for .NET that focuses on simplicity, ease-of-use, and direct control over SQL queries. It is often chosen when performance is a priority or when a more low-level approach to data access is required. Entity Framework Core is a full-fledged ORM that provides a comprehensive feature set, with built-in support for change tracking, identity management, migrations, and LINQ