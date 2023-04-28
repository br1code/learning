# Lazy loading, Eager loading & Explicit loading

In Entity Framework Core (EF Core), loading refers to the process of retrieving data from the database and populating the navigation properties of your entities. There are three primary loading strategies: Lazy Loading, Eager Loading, and Explicit Loading. Each strategy has its own advantages and trade-offs depending on the use case.

1. Lazy Loading:

Lazy Loading is an on-demand loading strategy, where related data is automatically loaded from the database only when the navigation property is accessed for the first time. This can be convenient because you don't need to specify upfront which related data should be retrieved.

However, Lazy Loading can lead to performance issues due to the N+1 problem, where multiple queries are issued to the database when iterating over a collection of entities and accessing their related data.

To enable Lazy Loading in EF Core, you need to install the `Microsoft.EntityFrameworkCore.Proxies` package and configure your DbContext to use lazy loading proxies:

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder.UseLazyLoadingProxies().UseSqlServer("your_connection_string");
}
```

Then, mark the navigation properties as `virtual`:

```csharp
public class Blog
{
    // ...

    public virtual List<Post> Posts { get; set; }
}
```

2. Eager Loading:

Eager Loading is a loading strategy where related data is retrieved from the database along with the main entities in a single query, using the `Include` and `ThenInclude` methods. This can help avoid the N+1 problem and minimize the number of round-trips to the database.

However, Eager Loading can retrieve more data than necessary if you don't carefully select which related data to include, leading to increased memory usage and slower performance.

Example:

```csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
                       .Include(b => b.Posts)
                       .ToList();
}
```

3. Explicit Loading:

Explicit Loading is a loading strategy where related data is explicitly loaded from the database using the `Load` method. This gives you fine-grained control over when and which related data is retrieved, allowing you to optimize performance based on your application's specific requirements.

However, Explicit Loading requires more manual management of the loading process, and it can also lead to the N+1 problem if not used carefully.

Example:

```csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog)
           .Collection(b => b.Posts)
           .Load();
}
```

## When to use each strategy

- Lazy Loading: Use Lazy Loading when you want to automatically load related data on-demand, without specifying upfront which navigation properties to populate. Be cautious of potential performance issues due to the N+1 problem.
- Eager Loading: Use Eager Loading when you know in advance which related data you need, and you want to minimize database round-trips by retrieving all data in a single query. Be mindful of the amount of data being retrieved to avoid performance issues.
- Explicit Loading: Use Explicit Loading when you want fine-grained control over when and which related data is loaded, or when you need to conditionally load related data based on specific criteria.

## Recommendations and best practices

- Avoid Lazy Loading by default, especially in large-scale applications or in scenarios where you need to iterate over a collection of entities and their related data, to prevent the N+1 problem.
- Use Eager Loading when you know in advance which related data you need and aim to minimize database round-trips. Be selective with the `Include` method to avoid retrieving unnecessary data.
- Use Explicit Loading when you need fine-grained control over the loading process or want to conditionally load related data based on specific criteria. Explicit Loading can be useful in scenarios where you need to apply filters or sort related data before loading it.

- Always be mindful of the potential performance implications of your chosen loading strategy. Monitor and analyze the generated SQL queries to ensure they are efficient and avoid common performance issues like the N+1 problem.

- Consider using projection queries with the `Select` method to retrieve only the required data from the database, especially when dealing with large datasets or complex object graphs. This can help reduce the amount of data being transferred and improve performance.

Example:

```csharp
using (var context = new BloggingContext())
{
    var blogPosts = context.Blogs
                           .Where(b => b.BlogId == 1)
                           .Select(b => new
                           {
                               b.BlogId,
                               b.Url,
                               Posts = b.Posts.Where(p => p.Author == "John Doe")
                           })
                           .ToList();
}
```

In summary, Lazy Loading, Eager Loading, and Explicit Loading are the three primary loading strategies in EF Core, each with its own advantages and trade-offs. Select the most appropriate strategy based on your application's specific requirements, and always be mindful of the potential performance implications. Use projection queries to minimize the amount of data being transferred and optimize performance when necessary.

## N + 1 Problem

The N+1 problem is a common performance issue that occurs when retrieving related data from a database using an ORM like Entity Framework Core. The problem arises when a separate database query is issued for each entity in a collection and its related data, resulting in a total of N+1 queries, where N is the number of entities in the collection.

Simple Explanation:

Imagine you have a blog application with a list of blog posts and their respective authors. To display this list, you first query the database for all the blog posts (1 query). Then, for each blog post, you issue another query to fetch the author information (N queries, where N is the number of blog posts). This results in a total of N+1 queries, which can lead to poor performance and slow page load times.

Deep Explanation:

The N+1 problem is a consequence of inefficient data retrieval strategies, often caused by lazy loading or manual loading of related data. In these cases, the ORM issues separate queries to retrieve the primary entities and their related data, causing unnecessary round-trips to the database and increased latency.

Consider the following example using EF Core and Lazy Loading:

```csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();

    foreach (var blog in blogs)
    {
        Console.WriteLine($"Blog: {blog.Url}");

        // This line triggers a separate query for each blog's related posts due to Lazy Loading
        foreach (var post in blog.Posts)
        {
            Console.WriteLine($"- {post.Title}");
        }
    }
}
```

In this example, the first query retrieves all the blogs. Then, for each blog, a separate query is issued to fetch its related posts, resulting in a total of N+1 queries.

To avoid the N+1 problem, you can use Eager Loading or projection queries to retrieve the primary entities and their related data in a single query, minimizing database round-trips and improving performance.

Example using Eager Loading:

```csharp
using (var context = new BloggingContext())
{
    // Retrieve all blogs along with their related posts in a single query
    var blogs = context.Blogs.Include(b => b.Posts).ToList();

    foreach (var blog in blogs)
    {
        Console.WriteLine($"Blog: {blog.Url}");

        // No separate query is issued for the related posts
        foreach (var post in blog.Posts)
        {
            Console.WriteLine($"- {post.Title}");
        }
    }
}
```

In summary, the N+1 problem is a performance issue that occurs when separate database queries are issued for each entity in a collection and its related data, resulting in a total of N+1 queries. To avoid this problem, use Eager Loading or projection queries to retrieve the primary entities and their related data in a single query, minimizing database round-trips and improving performance.