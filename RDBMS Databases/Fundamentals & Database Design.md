# Fundamentals & Database Design

RDBMS (Relational Database Management System) is a type of database management system that stores data in a structured format using tables. These tables consist of rows and columns, where each row represents a record and each column represents a field or an attribute of that record. The relational model allows data to be easily organized, accessed, and managed by defining relationships between tables based on common attributes.

Here are the fundamentals of RDBMS databases:

1. **Tables:** A table is a collection of related data organized in rows and columns. It represents an entity, such as a person, a product, or an order, and its attributes. Each row is called a record or tuple, and each column is called a field or attribute.

2. **Keys:** Keys are used to identify and establish relationships between tables. There are several types of keys, such as primary keys, foreign keys, and unique keys. A primary key is a column or a combination of columns that uniquely identifies each row in a table. A foreign key is a column or a combination of columns that refers to the primary key of another table, establishing a relationship between the two tables.

3. **Relationships:** Relationships between tables are the core of the relational model. They allow you to associate and retrieve data from multiple tables based on common attributes. There are three main types of relationships: one-to-one, one-to-many, and many-to-many.

4. **Schema:** A schema is a collection of database objects, such as tables, views, stored procedures, and triggers. It defines the structure of the database, including the tables, their columns, and the relationships between them. A schema also defines constraints, which enforce data integrity and consistency.

5. **Normalization:** Database normalization is the process of organizing the tables and their relationships in a way that reduces data redundancy and improves data integrity. The goal is to create a database design that adheres to a set of well-defined normal forms, which describe specific criteria for structuring tables and relationships.

6. **SQL:** SQL (Structured Query Language) is a standard language used to interact with RDBMS databases. It allows you to create, retrieve, update, and delete data in a relational database. SQL supports various operations, such as SELECT, INSERT, UPDATE, DELETE, and JOIN, which help you perform tasks like querying data, modifying data, and defining relationships between tables.

7. **Indexes:** An index is a database object that speeds up data retrieval by providing a fast and efficient way to locate records based on specific column values. Indexes can be created on one or more columns of a table to optimize query performance. However, they come with a trade-off, as they can slow down data modification operations like INSERT, UPDATE, and DELETE.

8. **Transactions:** A transaction is a sequence of one or more database operations that must be executed as a single unit of work. Transactions ensure data consistency and integrity by adhering to the ACID properties: Atomicity, Consistency, Isolation, and Durability. This means that either all operations in a transaction are successfully completed, or none are, and the database remains in a consistent state.

9. **Concurrency Control:** Concurrency control is a mechanism used by RDBMS databases to manage simultaneous access to the database by multiple users while maintaining data consistency and integrity. Techniques such as locking, versioning, and optimistic concurrency control help prevent conflicts and ensure that the database remains in a consistent state even when multiple users are accessing and modifying data at the same time.

When designing a relational database, it's essential to consider factors like normalization, data integrity, performance, and maintainability. By understanding these fundamentals and following best practices, you can create an efficient and well-structured database that meets your application's needs.