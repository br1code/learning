# Indexes

## What are they and how are they actually stored?

An index in a database is a data structure that allows the DBMS to quickly locate rows in a table based on the values in one or more columns. Indexes are designed to speed up query performance but come at the cost of additional storage space and potential overhead for data modification operations. To understand how an index works and how it is stored, let's explore the two most common types of index structures: B-Tree and Bitmap.

1. **B-Tree Indexes:**

The B-Tree (Balanced Tree) index is the most common index structure used by many relational databases like PostgreSQL, MySQL, and SQL Server. A B-Tree index is a balanced, sorted tree data structure where each node contains a set of keys and pointers to child nodes. The tree is balanced because all leaf nodes are at the same depth, which ensures efficient search, insert, and delete operations.

The B-Tree index structure comprises three types of nodes:

- Root node: The top node in the tree, which points to one or more child nodes.
- Internal nodes: The intermediate nodes in the tree, which contain keys and pointers to other child nodes.
- Leaf nodes: The lowest level nodes in the tree, which contain the indexed column values and pointers (row identifiers) to the corresponding rows in the table.

In a B-Tree index, the keys are stored in a sorted order, and each internal node points to a range of keys in its child nodes. When searching for a specific key, the DBMS starts at the root node and follows the pointers through the internal nodes until it reaches the leaf node containing the desired key and its corresponding row pointer.

2. **Bitmap Indexes:**

Bitmap indexes are another type of index structure that is used primarily in databases with a low cardinality of indexed columns (i.e., columns with a small number of distinct values). A bitmap index uses a series of bitmaps (arrays of bits) to represent the presence or absence of each distinct value in the indexed column for each row in the table.

In a bitmap index, there is one bitmap for each distinct value in the indexed column. Each bit in the bitmap represents a single row in the table, and the position of the bit corresponds to the row identifier. A bit is set to 1 if the row contains the corresponding value for that bitmap, and 0 otherwise.

Bitmap indexes are highly compressed and can be very efficient for certain types of queries, such as searching for a specific value or performing bitwise operations to combine multiple indexed columns' values.

**Storage:**

Indexes are typically stored on disk, separate from the actual table data. The storage format and location depend on the specific database system being used. For example, in PostgreSQL, indexes are stored as separate files in the same tablespace as the table they belong to. In SQL Server, indexes are stored as objects within the same database file as the table data.

When an index is created, the database system builds the index structure based on the specified columns and stores it on disk. As data is inserted, updated, or deleted in the table, the database system updates the index to maintain its accuracy and efficiency.

Understanding the internal structure and storage of indexes helps you better grasp how they work and why they can significantly improve query performance in a relational database. However, it's essential to use them judiciously, as they also come with storage and maintenance overhead.

## Indexes in SQL

Indexes in SQL are database objects that help improve the performance of queries by providing a more efficient way of accessing data in tables. They work similarly to the index of a book, allowing the database management system (DBMS) to quickly find the rows associated with a specific value or a range of values.

1. **Single-Column Index:** A single-column index is an index created on a single column of a table. It is the most common type of index and is useful when querying data based on the values in one specific column.

Example: Create a single-column index on the 'email' column of the 'customers' table:

```sql
CREATE INDEX idx_customers_email ON customers(email);
```

Use case: Single-column indexes are helpful when you have queries that filter or sort data based on a single column, such as looking up a customer by their email address.

2. **Multi-Column Index (Composite Index):** A multi-column index, also known as a composite index, is an index created on two or more columns of a table. It is useful when querying data based on the combination of values in multiple columns.

Example: Create a multi-column index on the 'first_name' and 'last_name' columns of the 'customers' table:

```sql
CREATE INDEX idx_customers_first_last_name ON customers(first_name, last_name);
```

Use case: Multi-column indexes are beneficial when you have queries that filter or sort data based on multiple columns, such as searching for customers with a specific first and last name combination.

3. **Unique Index:** A unique index is an index that enforces the uniqueness constraint on the indexed column(s). It ensures that no two rows in the table have the same values for the indexed columns. A unique index can be created on a single column or multiple columns.

Example: Create a unique index on the 'email' column of the 'customers' table:

```sql
CREATE UNIQUE INDEX idx_customers_email ON customers(email);
```

Use case: Unique indexes are useful when you want to enforce uniqueness on columns other than the primary key, such as ensuring that no two customers have the same email address.

4. **Clustered Index:** A clustered index determines the physical order of data storage in a table. In a table with a clustered index, the rows are stored on disk in the same order as the index. Because of this, there can only be one clustered index per table.

Clustered indexes are not supported by all database systems. For example, PostgreSQL does not have clustered indexes, while SQL Server automatically creates a clustered index on the primary key unless specified otherwise.

Use case: Clustered indexes are useful when you have queries that retrieve data in the same order as the index, such as fetching records in chronological order based on a 'created_at' column.

5. **Non-Clustered Index:** A non-clustered index is an index that does not affect the physical order of data storage in a table. Instead, it maintains a separate structure that stores a copy of the indexed column(s) and a pointer to the corresponding row in the table. A table can have multiple non-clustered indexes.

In PostgreSQL, all indexes are non-clustered by default.

Use case: Non-clustered indexes are helpful when you have queries that filter, sort, or join data based on columns that are not part of the clustered index (if any). They can speed up queries without altering the underlying table structure.

## Examples and common scenarios

Here are three scenarios where using indexes can help improve query performance:

**Scenario 1: Filtering on frequently searched columns**

Suppose you have an e-commerce website with a `products` table containing information about the products available for sale. The table has columns like `id`, `name`, `category`, `price`, and `is_active`. Users often search for products based on their category and whether they are active (available for sale). 

In this case, you could create indexes on the `category` and `is_active` columns to speed up the search queries.

```sql
CREATE INDEX idx_products_category ON products(category);
CREATE INDEX idx_products_is_active ON products(is_active);
```

**Scenario 2: Sorting on commonly used columns**

Imagine you have a blog website with a `posts` table containing columns like `id`, `title`, `author_id`, `content`, `created_at`, and `updated_at`. Users often view the posts sorted by their creation date or update date.

To speed up these queries, you could create indexes on the `created_at` and `updated_at` columns:

```sql
CREATE INDEX idx_posts_created_at ON posts(created_at);
CREATE INDEX idx_posts_updated_at ON posts(updated_at);
```

**Scenario 3: Optimizing JOIN operations**

Suppose you have a project management application with `tasks` and `users` tables. The `tasks` table has columns like `id`, `name`, `description`, `status`, and `assigned_to_user_id`, and the `users` table has columns like `id`, `name`, and `email`. Users frequently query for tasks and their assignees.

In this case, you could create an index on the `assigned_to_user_id` column in the `tasks` table and the `id` column in the `users` table (if not already indexed as primary keys) to speed up JOIN operations:

```sql
CREATE INDEX idx_tasks_assigned_to_user_id ON tasks(assigned_to_user_id);
```

**Process of figuring out when to use indexes and how:**

1. Identify performance bottlenecks: Analyze slow-running queries using tools like query execution plans, database logs, or profiling tools. Find out which queries are taking a long time to execute and which parts of the query are causing the bottleneck (e.g., filtering, sorting, or joining).

2. Examine the table schema and data distribution: Review the columns used in the slow queries' conditions and analyze the data distribution in those columns. High cardinality columns (columns with many distinct values) are usually good candidates for indexing.

3. Consider the query patterns: Evaluate how frequently the identified columns are used in different queries. It's more beneficial to create indexes on columns that are frequently used across various queries.

4. Create and test the indexes: Create the appropriate index(es) based on your analysis and test the performance of the slow queries with the new indexes in place. Monitor the query execution time and compare it with the previous execution time to evaluate the improvement.

5. Monitor and maintain the indexes: Continuously monitor the performance of your queries and the health of your indexes. Update, rebuild, or reorganize indexes as needed to ensure optimal performance.

Remember, while indexes can significantly improve query performance, they come with storage and maintenance overhead. Use them judiciously and focus on optimizing the most critical and frequently executed queries in your application.

## Tips and best practices

1. Be selective when creating indexes. Although they can improve query performance, they also consume storage space and can slow down insert, update, and delete operations. Create indexes only on columns that are frequently used in WHERE clauses, JOIN conditions, or ORDER BY clauses.

2. Keep your indexes narrow. The narrower (fewer columns) an index is, the less space it occupies, and the faster it is for the DBMS to use. Try to limit the number of columns in a composite index to the minimum necessary for the most important queries.

3. Be mindful of the column order in multi-column indexes. The order of columns in a composite index matters, as it affects the types of queries the index can support efficiently. When creating a multi-column index, place the most selective columns (those with the highest number of distinct values) first, as this can help optimize the use of the index for a broader range of queries.

4. Regularly monitor and analyze query performance. Use tools like execution plans and database profiling to identify slow-running queries and determine if additional indexes or index modifications are needed to improve performance.

5. Periodically review and maintain your indexes. Over time, indexes can become fragmented or outdated, leading to suboptimal performance. Regularly check the health of your indexes, and consider rebuilding or reorganizing them if necessary. Also, remove any unused or redundant indexes to reduce storage overhead and maintenance costs.

6. Consider using covering indexes to optimize specific queries. A covering index is a non-clustered index that includes all the columns needed to satisfy a specific query, so the DBMS does not have to access the underlying table. This can significantly speed up query execution. However, covering indexes can consume more storage space, so use them judiciously and only for critical queries.

---

In summary, indexes are a crucial aspect of optimizing SQL database performance. By understanding the different types of indexes, their use cases, and best practices, you can design and maintain efficient database structures that support fast and responsive applications.