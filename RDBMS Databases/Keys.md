# Keys

Keys in SQL are essential elements of a relational database that help establish relationships between tables and ensure data integrity. There are several types of keys in SQL:

1. **Primary Key:** A primary key is a unique identifier for each row in a table. It must contain unique values and cannot be NULL. Each table can have only one primary key, which can consist of one or multiple columns.

Example: Define a primary key on the 'customer_id' column in the 'customers' table:

```sql
CREATE TABLE customers (
  customer_id SERIAL PRIMARY KEY,
  first_name VARCHAR(255) NOT NULL,
  last_name VARCHAR(255) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL
);
```

Use case: Primary keys are used to uniquely identify records in a table. They help maintain data integrity and serve as a reference point for foreign keys in other tables.

2. **Foreign Key:** A foreign key is a column (or a set of columns) in one table that refers to the primary key in another table. Foreign keys help maintain referential integrity by enforcing a relationship between two tables.

Example: Define a foreign key on the 'customer_id' column in the 'orders' table, referencing the 'customer_id' primary key in the 'customers' table:

```sql
CREATE TABLE orders (
  order_id SERIAL PRIMARY KEY,
  customer_id INTEGER NOT NULL,
  order_date DATE NOT NULL,
  total NUMERIC(10, 2) NOT NULL,
  FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```

Use case: Foreign keys are used to create relationships between tables and ensure that related data is consistent across tables. They prevent you from adding an order with a non-existent customer, or deleting a customer while related orders still exist.

3. **Unique Key:** A unique key is a constraint that ensures all values in a column (or a set of columns) are unique. Unlike primary keys, unique keys can have NULL values, and a table can have multiple unique keys.

Example: Define a unique key on the 'email' column in the 'customers' table:

```sql
CREATE TABLE customers (
  customer_id SERIAL PRIMARY KEY,
  first_name VARCHAR(255) NOT NULL,
  last_name VARCHAR(255) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL
);
```

Use case: Unique keys are used to enforce uniqueness on columns that are not primary keys, ensuring that no two rows have the same value in the specified column(s). For example, you can use a unique key to prevent customers from having the same email address.

4. **Composite Key:** A composite key is a key that consists of two or more columns in a table. Composite keys can be used as primary keys, foreign keys, or unique keys, depending on the use case.

Example: Define a composite primary key on the 'student_id' and 'course_id' columns in the 'enrollments' table:

```sql
CREATE TABLE enrollments (
  student_id INTEGER NOT NULL,
  course_id INTEGER NOT NULL,
  enrollment_date DATE NOT NULL,
  PRIMARY KEY (student_id, course_id),
  FOREIGN KEY (student_id) REFERENCES students(student_id),
  FOREIGN KEY (course_id) REFERENCES courses(course_id)
);
```

Use case: Composite keys are useful when a single column is not sufficient to uniquely identify a row or enforce a relationship between tables. In the example above, a student can enroll in multiple courses, and a course can have multiple students enrolled, so a composite primary key is used to uniquely identify each enrollment record.

These key types play a crucial role in designing and maintaining a well-structured and efficient relational database. By using keys effectively, you can ensure data integrity, maintain relationships between tables, and optimize query performance.

5. **Candidate Key:** A candidate key is a column (or a set of columns) that can uniquely identify each row in a table. Candidate keys are potential primary keys, as they all satisfy the uniqueness constraint required for primary keys. A table can have multiple candidate keys, but only one of them becomes the primary key.

Example: In the 'customers' table, both 'customer_id' and 'email' columns can uniquely identify each customer, so they are candidate keys:

```sql
CREATE TABLE customers (
  customer_id SERIAL,
  first_name VARCHAR(255) NOT NULL,
  last_name VARCHAR(255) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL
);
```

Use case: Candidate keys help you identify possible primary keys for your tables. While designing a database, it's essential to recognize all candidate keys and choose the most appropriate one as the primary key based on factors such as performance, storage, and ease of use.

6. **Surrogate Key:** A surrogate key is an artificial or system-generated unique identifier for each row in a table. Surrogate keys are usually implemented as auto-incrementing integers or globally unique identifiers (GUIDs). They have no business meaning and are used solely for the purpose of identifying a row uniquely.

Example: Define a surrogate key as an auto-incrementing 'customer_id' column in the 'customers' table:

```sql
CREATE TABLE customers (
  customer_id SERIAL PRIMARY KEY,
  first_name VARCHAR(255) NOT NULL,
  last_name VARCHAR(255) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL
);
```

Use case: Surrogate keys are useful when there is no suitable natural key (a key based on the actual data) to serve as a primary key. They can improve query performance, simplify application logic, and make it easier to manage relationships between tables.

7. **Natural Key:** A natural key is a column (or a set of columns) that can uniquely identify each row in a table based on the actual data. Natural keys have a real-world meaning and are not system-generated like surrogate keys.

Example: In the 'customers' table, the 'email' column can uniquely identify each customer and has real-world meaning, so it is a natural key:

```sql
CREATE TABLE customers (
  customer_id SERIAL PRIMARY KEY,
  first_name VARCHAR(255) NOT NULL,
  last_name VARCHAR(255) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL
);
```

Use case: Natural keys can be useful when you want to enforce uniqueness based on real-world data or when you need to establish relationships between tables without adding artificial identifiers. However, natural keys can sometimes change or become non-unique, which can lead to maintenance challenges.

Understanding the different types of keys in SQL and their uses is essential for designing and maintaining an efficient and well-structured relational database. Using keys effectively helps ensure data integrity, maintain relationships between tables, and optimize query performance.