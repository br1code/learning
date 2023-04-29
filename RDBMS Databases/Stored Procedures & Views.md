# Stored Procedures & Views

Stored Procedures and Views are two important database objects in SQL that help optimize query performance and encapsulate complex logic.

## Stored Procedures

A stored procedure is a precompiled group of SQL statements that are stored in the database. Stored procedures can accept parameters, perform complex operations (e.g., calculations, looping, conditional logic), and return results. They are particularly useful for encapsulating complex or repetitive tasks, improving performance, and enforcing security.

Here's an example of a stored procedure that retrieves the total number of tasks per user from a `tasks` table:

```sql
-- In PostgreSQL
CREATE OR REPLACE FUNCTION get_task_count_by_user()
RETURNS TABLE (user_id INT, task_count BIGINT) AS $$
BEGIN
  RETURN QUERY
  SELECT assigned_to_user_id AS user_id, COUNT(*) AS task_count
  FROM tasks
  GROUP BY assigned_to_user_id;
END;
$$ LANGUAGE plpgsql;
```

```sql
-- In SQL Server
CREATE PROCEDURE get_task_count_by_user
AS
BEGIN
  SELECT assigned_to_user_id AS user_id, COUNT(*) AS task_count
  FROM tasks
  GROUP BY assigned_to_user_id;
END;
```

To execute the stored procedure:

```sql
-- In PostgreSQL
SELECT * FROM get_task_count_by_user();
```

```sql
-- In SQL Server
EXEC get_task_count_by_user;
```

## Views

A view is a virtual table that represents the result of a stored query. Views do not store any data themselves; instead, they reference the underlying tables and provide a consistent interface to access specific data. Views can be used to encapsulate complex joins, aggregations, or filtering logic, and provide security by limiting access to specific columns or rows.

Here's an example of a view that retrieves the number of tasks per user from a `tasks` table and a `users` table:

```sql
-- In PostgreSQL and SQL Server
CREATE VIEW task_count_by_user AS
  SELECT u.id AS user_id, u.name AS user_name, COUNT(t.id) AS task_count
  FROM users u
  LEFT JOIN tasks t ON u.id = t.assigned_to_user_id
  GROUP BY u.id, u.name;
```

To query the view:

```sql
SELECT * FROM task_count_by_user;
```

## Comparison

Stored Procedures and Views both serve important purposes in SQL databases, but they have different characteristics and use cases. Here's a comparison between the two:

**Stored Procedures:**

1. **Functionality:** Stored procedures are precompiled groups of SQL statements that can perform complex operations, accept input parameters, and return results. They can include control structures like loops and conditional statements, allowing for more sophisticated logic.

2. **Performance:** Since stored procedures are precompiled, they can offer better performance, particularly for complex or repetitive tasks. The database server can optimize the execution plan, and the precompiled code reduces the overhead of parsing and compiling SQL statements each time they are executed.

3. **Security:** Stored procedures can enforce security by encapsulating operations and providing a controlled interface to interact with the data. By granting users access only to the stored procedures, you can limit their ability to perform unauthorized actions.

4. **Maintenance:** Stored procedures can simplify maintenance by encapsulating complex or repetitive tasks. By centralizing the logic, you can easily update or modify the code without having to change multiple queries throughout the application.

**Views:**

1. **Functionality:** Views are virtual tables that represent the result of a stored query. They do not store any data themselves, but rather reference the underlying tables. Views can encapsulate complex joins, aggregations, or filtering logic, but they cannot include control structures or accept input parameters.

2. **Performance:** Views do not offer the same performance benefits as stored procedures, because they are not precompiled. However, they can still provide some optimization by encapsulating complex logic or frequently used queries, allowing the database server to cache the execution plan.

3. **Security:** Views can enforce security by providing limited access to specific columns or rows in the underlying tables. By granting users access only to the views, you can control which data they can see and manipulate.

4. **Maintenance:** Views can simplify maintenance by encapsulating complex queries or joins and providing a consistent interface to access specific data. By centralizing the logic, you can easily update or modify the view definition without having to change multiple queries throughout the application.

In summary, stored procedures and views both help to encapsulate complex logic, improve performance, and enforce security in SQL databases. However, stored procedures are more suitable for situations requiring sophisticated operations, control structures, or input parameters, while views are better suited for encapsulating complex queries, joins, or filtering logic and providing a consistent interface to access specific data.

---

In summary, stored procedures and views are powerful tools for encapsulating complex logic, improving performance, and enforcing security in SQL databases. Stored procedures are precompiled groups of SQL statements that can accept parameters and perform complex operations, while views are virtual tables that represent the result of a stored query and provide a consistent interface to access specific data. By using these features effectively, you can optimize your database schema and make it easier to manage and maintain.