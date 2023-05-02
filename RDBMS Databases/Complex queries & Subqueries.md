# Complex queries & Subqueries

Complex queries and subqueries in SQL allow you to perform advanced data manipulation and retrieval operations by combining multiple tables, filtering, sorting, and aggregating data. These queries can help you answer more sophisticated questions and extract valuable insights from your data.

## Complex Queries

Complex queries often involve multiple tables, JOIN operations, and advanced filtering or aggregation. Here's an example of a complex query that retrieves a list of customers and their total orders, sorted by the total order amount:

```sql
SELECT c.CustomerId, c.FirstName, c.LastName, COUNT(o.OrderId) AS TotalOrders, SUM(o.Amount) AS TotalAmount
FROM Customers c
JOIN Orders o ON c.CustomerId = o.CustomerId
WHERE o.OrderDate >= '2021-01-01' AND o.OrderDate <= '2021-12-31'
GROUP BY c.CustomerId, c.FirstName, c.LastName
HAVING COUNT(o.OrderId) >= 5
ORDER BY SUM(o.Amount) DESC;
```

In this example:

- The `Customers` and `Orders` tables are joined on the `CustomerId` column.
- The `WHERE` clause filters orders by date range.
- The `GROUP BY` clause groups the results by customer.
- The `HAVING` clause filters the groups by the number of orders.
- The `ORDER BY` clause sorts the results by the total order amount.

## Subqueries

Subqueries are queries embedded within other queries, often used for filtering, aggregation, or data manipulation. Subqueries can be used in various parts of a query, such as the SELECT, FROM, WHERE, or HAVING clauses. They can be classified as scalar subqueries, inline views, or correlated subqueries.

1. **Scalar subqueries:** Scalar subqueries return a single value and can be used in expressions or conditions. Here's an example of a scalar subquery that retrieves the average order amount for a specific customer:

```sql
SELECT CustomerId, FirstName, LastName, (
    SELECT AVG(Amount)
    FROM Orders
    WHERE CustomerId = c.CustomerId
) AS AverageOrderAmount
FROM Customers c;
```

2. **Inline views:** Inline views are subqueries used in the FROM clause, allowing you to create a temporary table for use within the main query. Here's an example of an inline view that retrieves the total order amount for each customer:

```sql
SELECT c.CustomerId, c.FirstName, c.LastName, o.TotalAmount
FROM Customers c
JOIN (
    SELECT CustomerId, SUM(Amount) AS TotalAmount
    FROM Orders
    GROUP BY CustomerId
) o ON c.CustomerId = o.CustomerId;
```

3. **Correlated subqueries:** Correlated subqueries reference columns from the outer query and are executed once for each row in the outer query. Here's an example of a correlated subquery that retrieves customers with above-average order amounts:

```sql
SELECT c.CustomerId, c.FirstName, c.LastName, AVG(o.Amount) AS AverageOrderAmount
FROM Customers c
JOIN Orders o ON c.CustomerId = o.CustomerId
GROUP BY c.CustomerId, c.FirstName, c.LastName
HAVING AVG(o.Amount) > (
    SELECT AVG(Amount)
    FROM Orders
);
```

Complex queries and subqueries can be powerful tools for data analysis and manipulation in SQL, but they can also be challenging to write, read, and maintain. It's essential to use them judiciously, ensure proper indexing and performance optimization, and document your queries to help others understand your logic.