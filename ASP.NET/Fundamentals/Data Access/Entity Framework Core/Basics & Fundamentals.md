# Basics & Fundamentals

Entity Framework Core (EF Core) is an open-source, lightweight, extensible, and cross-platform Object-Relational Mapping (ORM) framework for .NET applications. It allows developers to interact with databases using .NET objects instead of writing SQL queries, thereby abstracting database operations and providing a more efficient way to manage data.

## Key Concepts

1. DbContext: The DbContext class is a crucial component in EF Core. It represents a session with the database, and allows you to perform CRUD (Create, Read, Update, Delete) operations. It also manages the connection, caching, and transaction handling.

2. DbSet: DbSet represents a collection of entities of a specific type, typically corresponding to a table in the database. It allows you to perform LINQ queries and other operations on the data.

3. Model: A model is a representation of the database schema using C# classes. Each class represents a table, and properties within the class represent columns in the table. EF Core can generate the database schema from your model using the Code-First approach, or create a model from an existing database using the Database-First approach.

4. Migration: Migration is a feature that enables you to evolve the database schema as your application evolves. EF Core can generate migration scripts based on changes in your model, which can then be applied to the database.

## Examples

Here are some code examples illustrating the basic usage of EF Core:

1. Define a model:

```csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

2. Create a DbContext:

```csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("Server=(localdb)\\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
    }
}
```

3. Query data using LINQ:

```csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
                       .Where(b => b.Url.Contains("example"))
                       .ToList();
}
```

4. Add a new blog:

```csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Url = "https://example.com" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```

5. Update a blog:

```csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Url = "https://new-example.com";
    context.SaveChanges();
}
```

6. Delete a blog:

```csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    context.Blogs.Remove(blog);
    context.SaveChanges();
}
```

## Entity Framework Core Queries in depth

Entity Framework Core (EF Core) simplifies data access by providing a high-level, expressive API for querying and manipulating data. The process of generating and executing queries to the database can be broken down into the following steps:

1. Writing LINQ Queries:

With EF Core, you write queries in C# using LINQ (Language Integrated Query). LINQ is a set of extension methods that enables you to write queries directly in your C# code. These queries are type-safe, expressive, and can take advantage of IntelliSense and compile-time checks.

Example:

```csharp
using (var context = new BloggingContext())
{
    var blogsWithPosts = context.Blogs
                                .Where(b => b.Posts.Count > 0)
                                .ToList();
}
```

2. Query Generation:

When you execute a LINQ query, EF Core's query generator is responsible for converting the LINQ expression tree into a SQL query that is compatible with your target database system. The query generator analyzes the LINQ expression tree, identifying the relevant entities, relationships, filters, projections, and aggregates, and generates the corresponding SQL statements.

The generated SQL query includes any necessary joins, filters, and projections based on the LINQ query, ensuring that only the required data is retrieved from the database.

In depth:

Sure! Let's walk through an example of how a LINQ query gets translated into a SQL query by EF Core. We'll start with a simple LINQ query and break down the process step-by-step.

Suppose we have the following entity classes for a blogging application:

```csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

Now, let's consider the following LINQ query:

```csharp
using (var context = new BloggingContext())
{
    var publishedBlogs = context.Blogs
                            .Where(b => b.Posts.Count > 0)
                            .Select(b => new { b.BlogId, b.Url, PostCount = b.Posts.Count })
                            .ToList();
}
```

This LINQ query retrieves all blogs that have at least one post and projects the results to an anonymous type containing the BlogId, Url, and PostCount.

Here's the step-by-step process of how EF Core translates this LINQ query into a SQL query:

1. **Analyze the LINQ expression tree:** EF Core examines the LINQ expression tree and identifies the relevant entities (Blogs and Posts), the filter condition (b.Posts.Count > 0), and the projection (selecting BlogId, Url, and PostCount).

2. **Create the SQL SELECT statement:** EF Core generates the SQL SELECT statement for the query, including the appropriate columns and aliases based on the projection.

3. **Create the SQL JOIN statement:** Since the query involves two related entities (Blogs and Posts), EF Core generates a SQL JOIN statement to combine the data from the two tables.

4. **Create the SQL WHERE clause:** EF Core generates the SQL WHERE clause based on the filter condition in the LINQ query (b.Posts.Count > 0). In this case, it uses a subquery to count the number of posts for each blog and compare it to 0.

5. **Create the SQL GROUP BY and HAVING clauses:** To accurately calculate the PostCount for each blog, EF Core generates a SQL GROUP BY clause to group the results by BlogId and Url. It also creates a HAVING clause to filter the grouped results based on the PostCount.

After these steps, EF Core generates the following SQL query (assuming a SQL Server database):

```sql
SELECT [b].[BlogId], [b].[Url], COUNT([p].[PostId]) AS [PostCount]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
GROUP BY [b].[BlogId], [b].[Url]
HAVING COUNT([p].[PostId]) > 0
```

This SQL query retrieves the desired information from the Blogs and Posts tables, applying the necessary joins, filters, and aggregations based on the original LINQ query. Once the query is executed and the results are returned, EF Core materializes the results into the specified anonymous type and returns the final list of publishedBlogs.

Note: You can see the generated SQL code by executing `string sql = query.ToQueryString();`

3. Database Providers and Command Execution:

EF Core relies on database providers to handle the specifics of connecting to and interacting with different database systems. Database providers are responsible for translating the generated SQL queries into commands that can be executed by the target database.

When EF Core is ready to execute a query, it communicates with the appropriate database provider. The provider then sends the SQL command to the database server, which processes the query and returns the results.

4. Materialization:

After the database provider retrieves the query results, EF Core maps the data back to C# objects based on the metadata and mapping information in the DbContext's Model. This process is called "materialization."

Materialization involves creating instances of the appropriate C# entity classes and populating their properties with the data from the query results. EF Core also handles any necessary type conversions and ensures that related entities are properly navigated and loaded based on the query.

5. Change Tracking (for queries that return tracked entities):

If the query returns entities that are tracked by the DbContext, EF Core will also set up change tracking for those entities. This allows EF Core to detect any changes made to the entities and generate the appropriate SQL commands (INSERT, UPDATE, or DELETE) when you call `SaveChanges` on the DbContext.

In summary, when you execute a query with EF Core, the process involves writing LINQ queries, generating the corresponding SQL queries, executing the SQL commands on the target database using database providers, materializing the results back into C# objects, and setting up change tracking for tracked entities. This high-level, expressive API simplifies data access and enables you to write type-safe, maintainable queries in your .NET applications.