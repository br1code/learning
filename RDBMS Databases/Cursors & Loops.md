# Cursors & Loops

Cursors and loops in SQL are used for iterative processing of data in a database. They allow you to perform operations on individual rows of a result set one at a time. Although set-based operations are generally preferred in SQL for better performance, cursors and loops can be helpful in certain scenarios where row-by-row processing is required.

**Note**: Cursors and loops are specific to procedural SQL languages like PL/SQL (for Oracle) and T-SQL (for SQL Server). The examples below will be using T-SQL (SQL Server), but the concepts are similar in other procedural SQL languages.

## Cursors

A cursor is a database object that allows you to retrieve rows from a result set and manipulate them one at a time. Cursors are typically used when you need to perform complex processing on each row of a result set, or when the order of processing is important.

Here's an example of a simple cursor in T-SQL:

```sql
DECLARE @CustomerId INT;
DECLARE @CustomerName NVARCHAR(100);

-- Declare the cursor
DECLARE CustomerCursor CURSOR FOR
SELECT Id, Name
FROM Customers;

-- Open the cursor and fetch the first row
OPEN CustomerCursor;
FETCH NEXT FROM CustomerCursor INTO @CustomerId, @CustomerName;

-- Loop through the rows of the result set
WHILE @@FETCH_STATUS = 0
BEGIN
    -- Perform operations on each row
    PRINT 'Customer ID: ' + CAST(@CustomerId AS NVARCHAR) + ', Name: ' + @CustomerName;

    -- Fetch the next row
    FETCH NEXT FROM CustomerCursor INTO @CustomerId, @CustomerName;
END;

-- Close and deallocate the cursor
CLOSE CustomerCursor;
DEALLOCATE CustomerCursor;
```

In this example, a cursor named `CustomerCursor` is declared and associated with a SELECT statement that retrieves customer IDs and names. The cursor is opened, and rows are fetched one at a time into variables. A WHILE loop iterates through the rows, printing the customer ID and name. Finally, the cursor is closed and deallocated.

## Loops

Loops in procedural SQL languages allow you to execute a block of code repeatedly based on a condition. There are different types of loops, such as WHILE loops, FOR loops, and cursor-based loops.

We've already seen a WHILE loop in the cursor example above. Here's an example of a WHILE loop that doesn't involve a cursor:

```sql
DECLARE @Counter INT = 1;
DECLARE @Result NVARCHAR(MAX) = '';

WHILE @Counter <= 10
BEGIN
    SET @Result = @Result + CAST(@Counter AS NVARCHAR) + ', ';
    SET @Counter = @Counter + 1;
END;

PRINT 'Numbers from 1 to 10: ' + @Result;
```

In this example, a counter variable is initialized with a value of 1, and a WHILE loop is used to concatenate the counter value to a result string. The loop iterates until the counter reaches 10, and the final result is printed.

Although cursors and loops can be useful in certain scenarios, they should be used sparingly in SQL. Set-based operations are generally more efficient and better suited for working with relational databases. Cursors and loops can be slow and resource-intensive, so it's important to use them only when necessary and to optimize their performance as much as possible.