# Week 3 – Databases (SQL + NoSQL)

## Week goals
- Build production-grade database fundamentals for backend work.
- Be interview-ready on normalization, indexing, transactions, and query optimization.
- Design schemas and choose between SQL and MongoDB based on use case.

## Day 15: MySQL normalization (1NF -> 3NF)
### Goal
Build a clean 3NF schema, explain functional dependencies, and justify trade-offs in an interview.

### Concepts
- **1NF (First Normal Form):** Atomic values, no repeating groups, unique rows via a primary key.
- **2NF (Second Normal Form):** In 1NF and every non-key column depends on the full composite key.
- **3NF (Third Normal Form):** In 2NF and no non-key column depends on another non-key column.

### Why normalization matters (anomalies)
- **Insert anomaly:** Cannot add a new product unless an order exists.
- **Update anomaly:** Same customer phone repeated across rows leads to inconsistent updates.
- **Delete anomaly:** Deleting the last order deletes customer info.

### Functional dependency quick rules
- **Partial dependency:** Non-key column depends on part of a composite key.
- **Transitive dependency:** Non-key column depends on another non-key column.

### Example: Normalize a single table
**Before normalization (repeating groups + mixed entities):**
```
OrderID | CustomerName | CustomerPhone | ProductID | ProductName | Quantity
1001    | Sara         | 0300-1111111  | P01       | Notebook    | 2
1001    | Sara         | 0300-1111111  | P02       | Pen         | 5
```

**1NF (atomic values, no repeating groups):**
```sql
CREATE TABLE order_items_1nf (
  order_id INT,
  customer_name VARCHAR(100),
  customer_phone VARCHAR(20),
  product_id VARCHAR(20),
  product_name VARCHAR(100),
  quantity INT,
  PRIMARY KEY (order_id, product_id)
);
```

**2NF (split partial dependencies):**
```sql
CREATE TABLE orders (
  order_id INT PRIMARY KEY,
  customer_name VARCHAR(100),
  customer_phone VARCHAR(20)
);

CREATE TABLE products (
  product_id VARCHAR(20) PRIMARY KEY,
  product_name VARCHAR(100)
);

CREATE TABLE order_items (
  order_id INT,
  product_id VARCHAR(20),
  quantity INT,
  PRIMARY KEY (order_id, product_id),
  FOREIGN KEY (order_id) REFERENCES orders(order_id),
  FOREIGN KEY (product_id) REFERENCES products(product_id)
);
```

**3NF (remove transitive dependencies):**
```sql
CREATE TABLE customers (
  customer_id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(100),
  phone VARCHAR(20)
);

ALTER TABLE orders
  ADD COLUMN customer_id INT,
  ADD FOREIGN KEY (customer_id) REFERENCES customers(customer_id);

ALTER TABLE orders
  DROP COLUMN customer_name,
  DROP COLUMN customer_phone;
```

### Checkpoint questions (interview-ready)
- What dependency is broken by 2NF in the example?
- What transitive dependency exists in the 2NF schema?
- Which anomalies are removed after 3NF?

### Joins (quick reference for normalized schemas)
**Description:**  
Joins combine data from multiple tables based on related columns. MySQL supports ANSI SQL join syntax but has optimizations for specific join types.

**Types Explained:**  
- **INNER JOIN:** Returns only matching rows from both tables  
- **LEFT JOIN:** Returns all rows from the left table + matches from right  
- **RIGHT JOIN:** Returns all rows from the right table + matches from left  
- **CROSS JOIN:** Cartesian product (all possible combinations)  
- **SELF JOIN:** Joins a table to itself  

**MySQL Examples:**
```sql
SELECT orders.order_id, customers.name
FROM orders
JOIN customers ON orders.customer_id = customers.customer_id;
```

```sql
SELECT customers.name, orders.order_date
FROM customers
LEFT JOIN orders ON customers.customer_id = orders.customer_id;
```

```sql
SELECT orders.order_id, customers.name
FROM orders
RIGHT JOIN customers ON orders.customer_id = customers.customer_id;
```

```sql
SELECT products.name, sizes.size
FROM products
CROSS JOIN sizes;
```

## Day 16: Indexing, transactions, isolation levels
- Indexing: B-Tree basics, composite indexes, covering indexes, selectivity.
- Transactions: ACID, autocommit, rollback scenarios.
- Isolation levels: Read Uncommitted, Read Committed, Repeatable Read, Serializable.
- Practice: Use EXPLAIN on two queries and explain the plan.

## Day 17: Query optimization and locks
- Query optimization: avoid SELECT *, limit result sets, use proper predicates.
- Locks: row vs table locks, shared vs exclusive, deadlocks and how they happen.
- Practice: Identify an N+1 query and propose a fix.

## Day 18: MongoDB schema design
- Schema design: embedded vs referenced documents.
- Cardinality: one-to-few, one-to-many, many-to-many.
- Practice: Model a product catalog with reviews and users.

## Day 19: Aggregation pipelines
- Core stages: $match, $group, $project, $sort, $lookup, $unwind.
- Practice: Build a pipeline to compute top-selling products by month.

## Day 20: SQL vs MongoDB trade-offs
- SQL: strong consistency, joins, structured schema, transactions.
- MongoDB: schema flexibility, document locality, horizontal scaling.
- Practice: Pick the right DB for logging, payments, and chat.

## Day 21: Design interview practice
- Design tasks:
  - Ecommerce order system (SQL schema + indexes).
  - Activity logging service (MongoDB + TTL indexes).
  - Messaging system (trade-offs and data model).
- Interview focus: explain design choices and trade-offs clearly.
