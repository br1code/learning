# Transactions & ACID

Transactions are a fundamental concept in database systems, ensuring data consistency and integrity even in the face of concurrent access and system failures. The ACID properties (Atomicity, Consistency, Isolation, and Durability) define the key characteristics of transactions.

**Transactions:**

A transaction is a sequence of one or more database operations (such as reading, inserting, updating, or deleting data) that are executed as a single unit of work. Transactions are used to ensure that data remains consistent and reliable, even when multiple users are accessing the database concurrently or when system failures occur.

**ACID properties:**

1. **Atomicity:** Atomicity ensures that a transaction is either fully completed or fully rolled back. If any part of the transaction fails, the entire transaction is aborted, and all changes made within the transaction are rolled back. This "all-or-nothing" behavior ensures that the database remains in a consistent state even if an error occurs.

Example:

Consider a banking system where you want to transfer money from one account to another. This operation consists of two steps: debiting the source account and crediting the destination account. If any of these steps fail, the entire transaction should be rolled back to ensure the correct balance in both accounts.

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1; -- Debit source account
UPDATE accounts SET balance = balance + 100 WHERE id = 2; -- Credit destination account
COMMIT;
```

2. **Consistency:** Consistency ensures that the database transitions from one consistent state to another after a transaction is executed. This means that all data constraints, rules, and triggers are enforced during the transaction, and the database remains in a valid state.

Example:

In a library system, each book can be borrowed by only one user at a time. When a user borrows a book, the transaction must ensure that the book is marked as borrowed and associated with the user. If another user tries to borrow the same book simultaneously, the transaction should enforce the constraint and prevent the operation.

3. **Isolation:** Isolation ensures that the concurrent execution of transactions does not interfere with one another. Each transaction should be isolated from others, so their intermediate states are not visible to other transactions. This prevents problems like dirty reads, non-repeatable reads, and phantom reads.

Example:

In an e-commerce system, two customers are trying to purchase the last item of a product. With proper isolation, the system processes the transactions sequentially, allowing only one customer to successfully purchase the item while informing the other customer that the item is out of stock.

4. **Durability:** Durability ensures that once a transaction is committed, its changes are permanent and will survive system failures. Typically, this is achieved by writing the transaction's data to non-volatile storage, like a hard disk or SSD.

Example:

In an inventory management system, when a shipment of new products is received, a transaction is used to update the stock levels. Durability ensures that this update is permanent, even if the system crashes immediately after the transaction is committed.

**Real-world examples:**

Transactions and ACID properties are crucial in various real-world applications, such as:

- Financial systems (banking, trading, payment processing): Ensuring the accuracy and consistency of financial transactions and preventing issues like double-spending or lost updates.
- E-commerce platforms: Maintaining inventory levels, processing orders, and managing user accounts and shopping carts.
- Reservation systems (hotels, airlines, car rentals): Ensuring that reservations are unique, available resources are correctly allocated, and overlapping reservations are prevented.
- Social media platforms: Managing user-generated content, maintaining relationships and privacy settings, and ensuring the correct delivery of messages and notifications.

By using transactions with ACID properties, these applications can provide a reliable and consistent experience for their users, even in the face of concurrent access, system failures, and other challenges.

In SQL, you can manage transactions using the following statements:

- `BEGIN`: Starts a new transaction.
- `COMMIT`: Commits the current transaction, making its changes permanent.
- `ROLLBACK`: Rolls back the current transaction, discarding all changes made within it.

Example:

Suppose you are building an e-commerce platform, and you need to ensure that when a customer places an order, both the order and its associated order items are saved to the database. You can use a transaction to achieve this:

```sql
-- Start the transaction
BEGIN;

-- Insert the order
INSERT INTO orders (customer_id, total_amount, order_date)
VALUES (1, 150.00, '2023-05-01');

-- Get the order ID (assuming the orders table has a serial/auto-increment primary key)
SELECT currval('orders_id_seq') INTO order_id;

-- Insert the order items
INSERT INTO order_items (order_id, product_id, quantity, price)
VALUES (order_id, 101, 2, 50.00),
       (order_id, 102, 1, 50.00);

-- Commit the transaction
COMMIT;
```

In this example, if any operation within the transaction fails (e.g., due to a constraint violation or a system error), the entire transaction will be rolled back, ensuring that the database remains in a consistent state.

In summary, transactions and ACID properties are essential concepts in database systems, providing a reliable and consistent way to manage data in the face of concurrent access and system failures. By understanding and applying these concepts, you can build robust and reliable applications that meet the demands of real-world use cases.