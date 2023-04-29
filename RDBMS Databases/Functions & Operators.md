# Functions & Operators

Functions and operators in SQL are used to perform various operations and transformations on the data stored in tables. Functions are predefined routines that take one or more input values and return a single value, while operators are symbols or keywords used to manipulate values or compare them.

## Functions

SQL functions can be categorized into several groups, including:

1. **Aggregate functions:** These functions perform calculations on a group of rows and return a single value. Some common aggregate functions are:

   - `COUNT()`: Counts the number of rows in a group.
   - `SUM()`: Calculates the sum of values in a group.
   - `AVG()`: Calculates the average of values in a group.
   - `MIN()`: Returns the minimum value in a group.
   - `MAX()`: Returns the maximum value in a group.

Example:

```sql
SELECT COUNT(id) AS total_products, AVG(price) AS average_price, MIN(price) AS lowest_price, MAX(price) AS highest_price
FROM products;
```

2. **Scalar functions:** These functions operate on a single value and return a single value. Some common scalar functions are:

   - `LOWER()`: Converts a string to lowercase.
   - `UPPER()`: Converts a string to uppercase.
   - `LENGTH()`: Returns the length of a string.
   - `ROUND()`: Rounds a numeric value to a specified number of decimal places.
   - `COALESCE()`: Returns the first non-null value from a list of values.

Example:

```sql
SELECT LOWER(name) AS lower_name, UPPER(name) AS upper_name, LENGTH(name) AS name_length, ROUND(price, 2) AS rounded_price
FROM products;
```

3. **Date and time functions:** These functions are used to perform operations on date and time values. Some common date and time functions are:

   - `NOW()`: Returns the current date and time.
   - `DATE()`: Extracts the date part from a date or datetime value.
   - `TIME()`: Extracts the time part from a datetime value.
   - `DATEDIFF()`: Calculates the difference between two date or datetime values.
   - `DATE_ADD()`: Adds an interval to a date or datetime value.

Example:

```sql
SELECT NOW() AS current_time, DATE(created_at) AS created_date, TIME(created_at) AS created_time, DATEDIFF(NOW(), created_at) AS days_since_creation
FROM products;
```

## Operators

Operators in SQL can be categorized into several groups, including:

1. **Arithmetic operators:** These operators are used to perform mathematical calculations. The main arithmetic operators are:

   - `+`: Addition
   - `-`: Subtraction
   - `*`: Multiplication
   - `/`: Division
   - `%`: Modulus (remainder)

Example:

```sql
SELECT price, price * 1.10 AS price_with_tax, price - discount AS discounted_price
FROM products;
```

2. **Comparison operators:** These operators are used to compare values and return a boolean result (TRUE, FALSE, or UNKNOWN). Some common comparison operators are:

   - `=`: Equal
   - `<>` or `!=`: Not equal
   - `<`: Less than
   - `>`: Greater than
   - `<=`: Less than or equal to
   - `>=`: Greater than or equal to

Example:

```sql
SELECT *
FROM products
WHERE price >= 100 AND price <= 200;
```

3. **Logical operators:** These operators are used to combine multiple conditions in a WHERE or HAVING clause. The main logical operators are:

   - `AND`: Returns TRUE if both conditions are TRUE.
   - `- `OR`: Returns TRUE if at least one of the conditions is TRUE.
   - `NOT`: Negates a condition, returning TRUE if the condition is FALSE, and vice versa.

Example:

```sql
SELECT *
FROM products
WHERE (price >= 100 AND price <= 200) OR (discount > 0);
```

4. **String operators:** These operators are used to manipulate string values. Some common string operators are:

   - `||` (in PostgreSQL) or `+` (in SQL Server): Concatenation operator that combines two strings into one.
   - `LIKE`: Compares a string to a pattern with wildcard characters (`%` for any number of characters, `_` for a single character).
   - `ILIKE` (in PostgreSQL): Case-insensitive version of the LIKE operator.
   - `SUBSTRING()`: Extracts a portion of a string.

Example:

```sql
-- In PostgreSQL
SELECT first_name || ' ' || last_name AS full_name, email
FROM users
WHERE email LIKE '%@example.com';

-- In SQL Server
SELECT first_name + ' ' + last_name AS full_name, email
FROM users
WHERE email LIKE '%@example.com';
```

In summary, functions and operators in SQL are essential tools for performing operations and transformations on data. Functions are predefined routines that take input values and return a single value, while operators are symbols or keywords used to manipulate or compare values. By using these tools effectively, you can build powerful and flexible queries to analyze and process your data.