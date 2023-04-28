# Change Tracker API

The Change Tracker API in Entity Framework Core (EF Core) is an essential component that monitors the changes made to entities in a DbContext. Whenever you modify, add, or delete an entity in a DbContext, the change tracker keeps track of these changes and applies them to the database when you call the SaveChanges() method.

The Change Tracker API provides a way to access and manipulate the tracked entities, as well as to control the tracking behavior. The following are the key features of the Change Tracker API:

1. EntityState: The EntityState is an enumeration that represents the state of an entity with respect to the change tracker. The possible EntityState values are:

- Added: The entity has been added to the context but not yet saved to the database.
- Unchanged: The entity is being tracked by the context but has not been modified.
- Modified: The entity has been modified, and the changes have not yet been saved to the database.
- Deleted: The entity has been deleted but not yet removed from the database.
- Detached: The entity is not being tracked by the context.

2. Accessing Tracked Entities: The ChangeTracker.Entries() method returns a collection of all the entities being tracked by the context, along with their EntityStates. You can filter this collection based on a specific entity type or EntityState.

Example:

```csharp
using (var context = new BloggingContext())
{
    var modifiedEntities = context.ChangeTracker.Entries()
                                  .Where(e => e.State == EntityState.Modified);
}
```

3. Changing EntityState: You can change the EntityState of an entity explicitly to control its tracking behavior. For example, you can detach an entity from the context to stop tracking its changes, or mark it as modified to force an update in the database.

Example:

```csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    context.Entry(blog).State = EntityState.Modified;
    context.SaveChanges();
}
```

4. Detecting Changes: The ChangeTracker.DetectChanges() method scans the tracked entities and updates their EntityState according to the modifications detected. EF Core calls this method automatically before executing SaveChanges(), but you can also call it manually to update the EntityState of entities on demand.

5. Auto-Detect Changes: The ChangeTracker.AutoDetectChangesEnabled property controls whether the context automatically detects changes in the tracked entities. Disabling auto-detection can improve performance in some scenarios, but you'll need to call DetectChanges() manually to update the EntityState of entities.

Example:

```csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.AutoDetectChangesEnabled = false;

    var blog = context.Blogs.Find(1);
    blog.Url = "https://new-example.com";

    context.ChangeTracker.DetectChanges();
    context.SaveChanges();
}
```

In summary, the Change Tracker API is an essential part of EF Core that manages entity states and tracks the changes made to entities within a DbContext. It provides methods and properties to access, modify and control the EntityState, allowing you to fine-tune the tracking behavior and optimize performance in your application.