# Restrictions & Validations

Restrictions and validations in SQL, such as in PostgreSQL, are mechanisms to ensure data integrity and consistency. By defining constraints and rules, you can ensure that the data stored in the database adheres to the requirements of your application. Some common types of restrictions and validations in SQL are:

1. **Primary Key constraint:** A primary key uniquely identifies each record in a table. Each table can have only one primary key, and it cannot contain NULL values. Primary keys are often used to establish relationships between tables.

Example:

```sql
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL
);
```

2. **Foreign Key constraint:** A foreign key establishes a relationship between two tables, ensuring that the data in the foreign key column(s) references a valid record in another table.

Example:

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customers(id),
    order_date DATE NOT NULL
);
```

3. **Unique constraint:** A unique constraint ensures that all values in a column (or a set of columns) are unique. This can be useful to prevent duplicate data, such as multiple users with the same email address.

Example:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    password VARCHAR(100) NOT NULL
);
```

4. **Check constraint:** A check constraint enforces a custom condition on a column (or a set of columns), ensuring that the data meets specific criteria.

Example:

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price NUMERIC CHECK (price >= 0),
    stock INTEGER CHECK (stock >= 0)
);
```

5. **NOT NULL constraint:** A NOT NULL constraint ensures that a column cannot contain NULL values. This is useful for columns that must always have a value.

Example:

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    hire_date DATE NOT NULL
);
```

6. **Domain constraints:** In PostgreSQL, you can create custom domains that define data types with specific constraints. This can be useful for enforcing consistency across different tables and columns.

Example:

```sql
-- Create a custom domain for non-negative integers
CREATE DOMAIN non_negative_int AS INTEGER CHECK (VALUE >= 0);

-- Use the custom domain in a table definition
CREATE TABLE inventory (
    id SERIAL PRIMARY KEY,
    product_id INTEGER REFERENCES products(id),
    quantity non_negative_int NOT NULL
);
```

By using these restrictions and validations, you can ensure that the data stored in your database is consistent, accurate, and adheres to the requirements of your application. This can help prevent data corruption, improve data quality, and simplify application logic by offloading some of the validation responsibilities to the database.