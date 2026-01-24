# Interview Preparation Topics

## 📌 Core Topics to Prepare

### 1. Object-Oriented Programming (OOP)
- Classes & Objects
- Inheritance, Polymorphism, Abstraction, Encapsulation
- Interfaces
- Virtual Functions & Abstract Classes
- Association and its Types

---

### 2. Data Structures & Algorithms (DSA)

#### Data Structures:
- Arrays, Strings, Linked Lists
- Stacks, Queues
- Trees (Binary Trees, BST, AVL, Heaps)
- Tree Balancing & Sorting
- Graphs (BFS, DFS, Dijkstra’s, MST)

#### Algorithms:
##### Searching Algorithms
- Linear Search
- Binary Search
- Depth First Search (DFS)
- Breadth First Search (BFS)

##### Sorting Algorithms
- Insertion Sort
- Heap Sort
- Selection Sort
- Merge Sort
- Quick Sort
- Counting Sort
- Bubble Sort
- Bucket Sort
- Radix Sort

##### Graph Algorithms
- Dijkstra's Algorithm
- Bellman-Ford Algorithm
- Floyd-Warshall Algorithm
- Topological Sort
- Kruskal's Algorithm

##### Array Algorithms
- Floyd's Cycle Detection
- Quick Select

##### Advanced Algorithms
- Huffman Coding (Compression)

##### Others
- Recursion & Backtracking
- Dynamic Programming
- Greedy Algorithms

---

### 3. Database Management (DB)
- SQL Queries (Joins, Subqueries, Aggregations)
- Normalization (1NF, 2NF, 3NF, BCNF)
- Indexing & Transactions (ACID Properties)
- NoSQL vs SQL (MongoDB vs PostgreSQL)

---

### 4. Computer Networks (CN)
- OSI & TCP/IP Models
- HTTP vs HTTPS, DNS, REST APIs
- TCP vs UDP (Connection-oriented vs Connectionless)
- IP Addressing (IPv4, Subnetting)

---

### 5. Coding & Problem-Solving
- Time & Space Complexity (Big-O Notation)
- Problem Patterns:
  - Two Pointers
  - Sliding Window
  - Tree/Graph Traversals
- Git Basics (Commit, Branching, Merge Conflicts)

---
---


# 📘 Interview Preparation Guide

---

# 1.🧠 Object-Oriented Programming (OOP)
Object-Oriented Programming (OOP) is a programming paradigm based on the concept of **“objects”**, which can contain data and code to manipulate the data. It aims to model real-world entities using classes and objects.

---

## Classes & Objects

**Explanation:**
- **Class**: A user-defined blueprint or prototype from which objects are created.
- **Object**: An instance of a class. It holds actual values for the attributes defined in the class.

**Example:**
```cpp
class Car {
public:
    string brand;
    void start() {
        cout << brand << " is starting!" << endl;
    }
};

int main() {
    Car myCar;
    myCar.brand = "Toyota";
    myCar.start();  // Output: Toyota is starting!
}
```

---

## Inheritance

**Explanation:**
Inheritance is the process by which one class (child/derived class) acquires the properties and behavior (methods) of another class (parent/base class).

**Example:**
```cpp
class Animal {
public:
    void sound() {
        cout << "Animal makes a sound" << endl;
    }
};

class Dog : public Animal {
public:
    void bark() {
        cout << "Dog barks" << endl;
    }
};

int main() {
    Dog d;
    d.sound();  // Inherited method
    d.bark();
}
```

---

## Polymorphism

**Explanation:**
Polymorphism allows one interface to be used for a general class of actions. The specific action is determined by the exact nature of the situation.

**Examples:**
**Compile-Time Polymorphism (Function Overloading):**
```cpp
class Print {
public:
    void show(int i) {
        cout << "Integer: " << i << endl;
    }

    void show(string s) {
        cout << "String: " << s << endl;
    }
};
```

**Run-Time Polymorphism (Virtual Functions):**
```cpp
class Base {
public:
    virtual void print() {
        cout << "Base class print" << endl;
    }
};

class Derived : public Base {
public:
    void print() override {
        cout << "Derived class print" << endl;
    }
};

int main() {
    Base* b = new Derived();
    b->print();  // Output: Derived class print
}
```

---

## Abstraction

**Explanation**
Abstraction hides complex implementation details and shows only the necessary features of an object.

**Example:**
```cpp
class AbstractDevice {
public:
    virtual void operate() = 0;  // Pure virtual function
};

class Fan : public AbstractDevice {
public:
    void operate() override {
        cout << "Fan is spinning" << endl;
    }
};
```

---

## Encapsulation

**Explanation:**
Encapsulation is the process of wrapping data and code into a single unit. Access modifiers control visibility.

**Example:**
```cpp
class Account {
private:
    int balance;

public:
    void setBalance(int b) {
        if (b >= 0) balance = b;
    }

    int getBalance() {
        return balance;
    }
};
```

---

## Virtual Functions & Abstract Classes

**Virtual Functions:**
Used to achieve run-time polymorphism by overriding functions in derived classes.

**Abstract Classes:**
Contain at least one pure virtual function. Cannot be instantiated.

**Example:**
```cpp
class Shape {
public:
    virtual void draw() = 0;  // Abstract method
};

class Circle : public Shape {
public:
    void draw() override {
        cout << "Drawing Circle" << endl;
    }
};
```

---

## Interfaces

**Explanation:**
In C++, an interface is implemented using an abstract class with only pure virtual functions.

**Example:**
```cpp
class IPrintable {
public:
    virtual void print() = 0;
};

class Document : public IPrintable {
public:
    void print() override {
        cout << "Printing document" << endl;
    }
};
```

---

## Association and its Types

**Explanation:**
Association represents relationships between classes.

**Types:**

| Type          | Description | Example |
|---------------|-------------|---------|
| Association   | General link between objects | A Doctor and a Patient |
| Aggregation   | "Has-a" - child can exist without parent | A Library has Books |
| Composition   | Strong "Has-a" - child cannot exist without parent | A House has Rooms |

**Example for Aggregation:**
```cpp
class Engine {
public:
    void start() {
        cout << "Engine starting..." << endl;
    }
};

class Car {
private:
    Engine* engine;  // Aggregation
public:
    Car(Engine* e) : engine(e) {}
    void startCar() {
        engine->start();
    }
};
```

**Code Example for Composition:**
```cpp
class Brain {
public:
    void think() {
        cout << "Thinking..." << endl;
    }
};

class Human {
private:
    Brain brain;  // Composition
public:
    void useBrain() {
        brain.think();
    }
};
```

---
---

# 2.📚 Data Structures & Algorithms (DSA) Guide

## 🧱 Data Structures

---

### Arrays
**Description**: Arrays are a collection of elements stored at contiguous memory locations. They allow fast access using an index.

**Example**:
```cpp
int arr[5] = {1, 2, 3, 4, 5};
cout << arr[2]; // Output: 3
```

---

### Strings
**Description**: Strings are arrays of characters ending with a null character (`\0`). C++ provides the `string` class for dynamic strings.

**Example**:
```cpp
string s = "Hello";
cout << s.length(); // Output: 5
```

---

### Linked Lists
**Description**: A linear data structure where elements are stored in nodes and each node points to the next.

**Example**:
```cpp
struct Node {
    int data;
    Node* next;
};
```

---

### Stacks
**Description**: Follows Last In First Out (LIFO) principle.

**Example**:
```cpp
stack<int> st;
st.push(10); st.pop();
```

---

### Queues
**Description**: Follows First In First Out (FIFO) principle.

**Example**:
```cpp
queue<int> q;
q.push(20); q.pop();
```

---

### Trees
**Description**: Hierarchical structure. BST keeps data ordered, AVL is self-balancing, and heaps maintain a heap property.

#### Binary Tree:
A tree where each node has at most two children, called left and right.

- Simple and foundational tree structure.
- Used in expression trees, hierarchical data models, etc.

#### Binary Search Tree:
A binary tree where the left child contains values less than the parent, and the right child contains values greater than the parent.

- Enables fast search, insert, and delete operations (average-case: O(log n)).
- Must be balanced for efficiency.

#### Balanced Tree:
A tree where the height difference between left and right subtrees is kept minimal to ensure performance.

- **AVL Tree:** Self-balancing BST, maintains balance using rotations after insert/delete.
- **Red-Black Tree:** Another self-balancing BST using coloring rules and rotations.

### Graphs
**Description**: Consists of nodes (vertices) and edges. Can be directed/undirected.

**Example**:
```cpp
vector<int> graph[100];
graph[0].push_back(1);
```

---

## 🏗️ Algorithms

---

### 🔍 Searching Algorithms

#### Linear Search
**Description**: Search each element until match is found.

**Example**:
```cpp
for (int i = 0; i < n; i++)
    if (arr[i] == x) return i;
```

---

#### Binary Search
**Description**: Efficient search on sorted array by dividing into halves.

**Example**:
```cpp
int binarySearch(int arr[], int l, int r, int x);
```

---

#### DFS (Depth First Search)
**Description**: Explore as far as possible along each branch.

**Example**:
```cpp
void DFS(int v) {
    visited[v] = true;
    for (int u : adj[v])
        if (!visited[u]) DFS(u);
}
```

---

#### BFS (Breadth First Search)
**Description**: Explore neighbor nodes level by level.

**Example**:
```cpp
queue<int> q;
q.push(start);
visited[start] = true;
```

---

### 📊 Sorting Algorithms

#### Insertion Sort
```cpp
for (int i = 1; i < n; i++) {
    int key = arr[i], j = i - 1;
    while (j >= 0 && arr[j] > key) arr[j + 1] = arr[j--];
    arr[j + 1] = key;
}
```

#### Heap Sort
Uses heap to extract elements in sorted order.

#### Selection Sort
Selects minimum and places in correct position.

#### Merge Sort
Divide and conquer sorting technique.

#### Quick Sort
Choose pivot, partition, and sort recursively.

#### Counting Sort
Used for small range integers.

#### Bubble Sort
```cpp
for (int i = 0; i < n-1; i++)
    for (int j = 0; j < n-i-1; j++)
        if (arr[j] > arr[j+1]) swap(arr[j], arr[j+1]);
```

#### Bucket Sort & Radix Sort
Used for integer or floating-point sorting with known range.

## Complexties

### 🔍 Searching Algorithms

| Algorithm         | Best Case  | Average Case | Worst Case  | Space Complexity | Notes                              |
|------------------|------------|--------------|-------------|------------------|-------------------------------------|
| **Linear Search** | O(1)       | O(n)         | O(n)        | O(1)             | Works on unsorted data              |
| **Binary Search** | O(1)       | O(log n)     | O(log n)    | O(1)             | Requires **sorted** array           |
| **DFS (Graph)**   | O(V + E)   | O(V + E)     | O(V + E)    | O(V)             | V = vertices, E = edges             |
| **BFS (Graph)**   | O(V + E)   | O(V + E)     | O(V + E)    | O(V)             | Uses a queue for traversal          |

---

### 🔃 Sorting Algorithms

| Algorithm         | Best Case  | Average Case | Worst Case  | Space Complexity | Stable? | Notes                                |
|------------------|------------|--------------|-------------|------------------|---------|---------------------------------------|
| **Insertion Sort**| O(n)       | O(n²)        | O(n²)       | O(1)             | ✅      | Good for small or nearly sorted data |
| **Heap Sort**     | O(n log n) | O(n log n)   | O(n log n)  | O(1)             | ❌      | Not stable, uses heap structure      |
| **Selection Sort**| O(n²)      | O(n²)        | O(n²)       | O(1)             | ❌      | Simple but inefficient               |
| **Merge Sort**    | O(n log n) | O(n log n)   | O(n log n)  | O(n)             | ✅      | Divide and conquer, stable           |
| **Quick Sort**    | O(n log n) | O(n log n)   | O(n²)       | O(log n)         | ❌      | Fast in practice, not stable         |
| **Counting Sort** | O(n + k)   | O(n + k)     | O(n + k)    | O(k)             | ✅      | Only for small-range integers        |
| **Bubble Sort**   | O(n)       | O(n²)        | O(n²)       | O(1)             | ✅      | Simple, inefficient for large data   |
| **Bucket Sort**   | O(n + k)   | O(n + k)     | O(n²)       | O(n + k)         | ✅      | Depends on distribution              |
| **Radix Sort**    | O(nk)      | O(nk)        | O(nk)       | O(n + k)         | ✅      | k = number of digits                 |

---

## 🌐 Graph Algorithms

### Dijkstra's Algorithm
Finds shortest paths from source to all nodes.

### Bellman-Ford Algorithm
Handles negative weights too.

### Floyd-Warshall Algorithm
All-pairs shortest paths.

### Topological Sort
Ordering of vertices in DAG.

### Kruskal's Algorithm
Minimum Spanning Tree using Union-Find.

---

## 📈 Array Algorithms

### Foyd’s Cycle Detection
Detects loop in linked list.

### Quick Select
Find kth smallest/largest element.

---

## 🔁 Advanced Algorithms

### Huffman Coding
Greedy compression algorithm.

---

## 🧠 Other Topics

### Recursion & Backtracking
Recursive solutions exploring all possibilities.

### Dynamic Programming
Solving problems by combining subproblem results.

### Greedy Algorithms
Locally optimal choice aiming for global optimum.

---
---

# 3.🗄️Complete MySQL Database Guide

## SQL Queries in MySQL

### Joins
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
JOIN customers ON orders.customer_id = customers.id;
```

```sql
SELECT customers.name, orders.order_date
FROM customers
LEFT JOIN orders ON customers.id = orders.customer_id;
```

```sql
SELECT orders.order_id, customers.customer_name
FROM orders
RIGHT JOIN customers ON orders.customer_id = customers.customer_id;
```

```sql
SELECT products.name, sizes.size
FROM products
CROSS JOIN sizes;
```

### Subqueries
**Description:**  
Queries nested inside other queries. MySQL processes them differently based on context (WHERE, FROM, SELECT).

**Performance Note:**  
MySQL 8.0+ optimizes subqueries better than older versions. Correlated subqueries can be slow in large datasets.

**MySQL Examples:**
```sql
-- WHERE clause subquery (MySQL executes this first)
SELECT name
FROM products
WHERE id IN (
  SELECT product_id FROM inventory WHERE quantity = 0
);
```

```sql
-- FROM clause subquery (MySQL calls this a "derived table")
SELECT avg_salaries.department, avg_salaries.avg_salary
FROM (
  SELECT department_id, AVG(salary) as avg_salary
  FROM employees
  GROUP BY department_id
) AS avg_salaries
WHERE avg_salaries.avg_salary > 50000;
```

```sql
-- EXISTS (MySQL stops processing when first match is found)
SELECT c.name
FROM customers c
WHERE EXISTS (
  SELECT 1 FROM orders
  WHERE customer_id = c.id AND total > 1000
);
```

### Aggregations
**Description:**  
Operations that summarize multiple rows. MySQL aggregation is optimized when using InnoDB tables with proper indexes.

**Key Functions:**  
- COUNT()  
- SUM()  
- AVG()  
- MIN()  
- MAX()  
- GROUP_CONCAT() (MySQL-specific string aggregation)  
- Window functions (MySQL 8.0+)

**MySQL Examples:**
```sql
-- Basic GROUP BY (MySQL requires all non-aggregated columns in GROUP BY)
SELECT department_id, COUNT(*) as emp_count, AVG(salary)
FROM employees
GROUP BY department_id;
```

```sql
-- HAVING (MySQL filters after aggregation)
SELECT product_id, SUM(quantity) as total_sold
FROM order_items
GROUP BY product_id
HAVING total_sold > 100;
```

```sql
-- GROUP_CONCAT (MySQL string aggregator)
SELECT department_id,
GROUP_CONCAT(DISTINCT last_name ORDER BY hire_date SEPARATOR '|') as employees
FROM employees
GROUP BY department_id;
```

## 2. Normalization in MySQL

### 1NF (First Normal Form)
**Description:**  
- Eliminates repeating groups  
- All attributes contain atomic (indivisible) values  
- Each record needs unique identifier (primary key)  

**Example:**  
Before 1NF (non-atomic values):  
```
OrderID | Products
--------|---------
1001    | Pen, Notebook, Pencil
```  
After 1NF:  
```
OrderID | Product
--------|--------
1001    | Pen
1001    | Notebook
1001    | Pencil
```

**1NF Implementation:**
```sql
-- Before (repeating groups)
CREATE TABLE orders (
  order_id INT PRIMARY KEY,
  products VARCHAR(255) -- "Item1,Item2,Item3"
);
```

```sql
-- After 1NF
CREATE TABLE order_items (
  order_id INT,
  product_id INT,
  quantity INT,
  PRIMARY KEY (order_id, product_id)
);
```

### 2NF (Second Normal Form)
**Description:**  
- Must be in 1NF  
- No partial dependency (all non-key attributes depend on entire primary key)  

**Example:**  
Before 2NF (partial dependency):  
```
OrderID | ProductID | ProductName | Quantity
--------|-----------|-------------|--------
1001    | P001      | Notebook    | 2
```  
After 2NF (split tables):  
Orders table:  
```
OrderID | ProductID | Quantity
--------|-----------|--------
1001    | P001      | 2
```  
Products table:  
```
ProductID | ProductName
----------|-------------
P001      | Notebook
```

**2NF Implementation:**
```sql
-- Before (partial dependency)
CREATE TABLE orders (
  order_id INT PRIMARY KEY,
  product_id INT,
  product_name VARCHAR(100), -- Depends only on product_id
  quantity INT
);
```

```sql
-- After 2NF
CREATE TABLE orders (
  order_id INT,
  product_id INT,
  quantity INT,
  PRIMARY KEY (order_id, product_id)
);

CREATE TABLE products (
  product_id INT PRIMARY KEY,
  product_name VARCHAR(100)
);
```

### 3NF (Third Normal Form)
**Description:**  
- Must be in 2NF  
- No transitive dependency (non-key attributes do not depend on other non-key attributes)  

**Example:**  
Before 3NF (transitive dependency):  
```
EmployeeID | Name | Department | DeptLocation
-----------|------|------------|-------------
101        | John | Sales      | Floor 1
```  
After 3NF:  
Employees table:  
```
EmployeeID | Name | DepartmentID
-----------|------|-------------
101        | John | 1
```  
Departments table:  
```
DepartmentID | Department | Location
-------------|------------|---------
1            | Sales      | Floor 1
```

**3NF Implementation:**
```sql
-- Before (transitive dependency)
CREATE TABLE employees (
  employee_id INT PRIMARY KEY,
  name VARCHAR(100),
  department VARCHAR(100),
  department_head VARCHAR(100) -- Depends on department
);
```

```sql
-- After 3NF
CREATE TABLE employees (
  employee_id INT PRIMARY KEY,
  name VARCHAR(100),
  department_id INT
);

CREATE TABLE departments (
  department_id INT PRIMARY KEY,
  name VARCHAR(100),
  head VARCHAR(100)
);
```

### BCNF (Boyce-Codd Normal Form)
**Description:**  
- Stricter version of 3NF  
- For any dependency A → B, A must be a superkey  

**Example:**  
Before BCNF (violation when a non-superkey determines another attribute):  
```
StudentID | Course | Instructor
----------|--------|-----------
S1        | Math   | ProfA
S1        | Physics| ProfB
```  
After BCNF:  
StudentCourses table:  
```
StudentID | CourseID
----------|---------
S1        | C1
S1        | C2
```  
CourseInstructors table:  
```
CourseID | Instructor
---------|-----------
C1       | ProfA
C2       | ProfB
```

**MySQL Implementation:**
```sql
-- Before Normalization (all in one table)
CREATE TABLE orders (
  order_id INT,
  customer_name VARCHAR(100), -- Repeats for same customer
  product_details JSON -- Contains multiple values
);
```

```sql
-- After 3NF (MySQL best practice)
CREATE TABLE customers (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(100) NOT NULL
) ENGINE=InnoDB;

CREATE TABLE products (
  id INT PRIMARY KEY,
  name VARCHAR(100),
  price DECIMAL(10,2)
) ENGINE=InnoDB;

CREATE TABLE orders (
  id INT PRIMARY KEY,
  customer_id INT,
  order_date DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (customer_id) REFERENCES customers(id)
  ON DELETE RESTRICT
) ENGINE=InnoDB;

CREATE TABLE order_items (
  order_id INT,
  product_id INT,
  quantity INT,
  PRIMARY KEY (order_id, product_id),
  FOREIGN KEY (product_id) REFERENCES products(id)
) ENGINE=InnoDB;
```

**1NF to BCNF Summary:**  
Normalization reduces data redundancy. MySQL does not enforce normalization rules automatically - it is the responsibility of the developer.

## 3. MySQL Indexing
**Description:**  
Indexes speed up searches but slow down writes. MySQL uses B-trees by default for most indexes.

**Types in MySQL:**  
- Primary Key (clustered index in InnoDB)  
- Secondary indexes  
- Full-text indexes (MyISAM/InnoDB)  
- Spatial indexes (for GIS data)

**Examples:**
```sql
-- Creating indexes (MySQL syntax)
ALTER TABLE customers
ADD INDEX idx_last_name (last_name(20)); -- Prefix index for VARCHAR
```

```sql
CREATE TABLE logs (
  id BIGINT AUTO_INCREMENT PRIMARY KEY,
  message TEXT,
  created_at DATETIME,
  INDEX idx_created (created_at)
) ENGINE=InnoDB;
```

```sql
-- EXPLAIN (MySQL query analysis tool)
EXPLAIN FORMAT=JSON
SELECT * FROM orders WHERE customer_id = 100;
```

## 4. MySQL Transactions
**Description:**  
Transactions ensure ACID compliance. MySQL default storage engine (InnoDB) fully supports transactions.

**ACID in MySQL:**  
- Atomicity: START TRANSACTION + COMMIT/ROLLBACK  
- Isolation: Configurable via SET TRANSACTION ISOLATION LEVEL  
- Durability: Guaranteed after COMMIT (unless hardware failure)

**Example:**
```sql
-- Transaction with error handling
DELIMITER //
BEGIN
  DECLARE EXIT HANDLER FOR SQLEXCEPTION
  BEGIN
    ROLLBACK;
    RESIGNAL;
  END;

  START TRANSACTION;

  UPDATE accounts SET balance = balance - 100
  WHERE user_id = 1;

  INSERT INTO transactions
  VALUES (NULL, 1, -100, NOW());

  COMMIT;
END //
DELIMITER ;
```

## 5. MySQL vs NoSQL

**Comparison Example:**
```sql
-- MySQL relational approach
SELECT u.name, p.title
FROM users u
JOIN posts p ON u.id = p.user_id
WHERE p.created_at > '2023-01-01';
```

```javascript
// Equivalent MongoDB
db.users.aggregate([
  {
    $lookup: {
      from: "posts",
      localField: "_id",
      foreignField: "user_id",
      as: "posts"
    }
  },
  { $unwind: "$posts" },
  { $match: { "posts.created_at": { $gt: ISODate("2023-01-01") } } },
  { $project: { name: 1, "posts.title": 1 } }
]);
```

**When to Use:**  
| Feature                     | SQL (Relational Databases)               | NoSQL (Non-Relational Databases)             |
|-----------------------------|------------------------------------------|----------------------------------------------|
| **Data Model**              | Structured, tabular (rows & columns)     | Flexible: key-value, document, graph, column |
| **Schema**                  | Fixed schema, predefined structure       | Dynamic schema, flexible structure           |
| **Scalability**             | Vertical (scale-up: more power to server)| Horizontal (scale-out: more servers)         |
| **ACID Compliance**         | Strong ACID compliance                   | Often relaxes ACID for performance/scalability|
| **Joins**                   | Supports complex joins                   | Limited or no joins                          |
| **Best Use Case**           | Structured data, complex queries         | Unstructured/semi-structured data, big data  |
| **Examples**                | MySQL, PostgreSQL, Oracle, SQL Server    | MongoDB, Cassandra, Redis, Couchbase         |
| **Query Language**          | SQL (Structured Query Language)          | Varies by database (e.g., JSON-like in MongoDB) |
| **Transactions**            | Strong and reliable                      | Varies – not always guaranteed               |
| **Flexibility**             | Less flexible due to strict schema       | Highly flexible and adaptable                |


---
---

# 4.📡 Computer Networks (CN) Detailed Guide

---

## 1. OSI and TCP/IP Models

### 📘 OSI Model (Open Systems Interconnection)
- **A conceptual framework** to understand and implement networking protocols in 7 layers.
- **Layers (Top to Bottom):**
  1. **Application** – Interface for end-user applications (e.g., browsers, email clients).
  2. **Presentation** – Translates data formats (e.g., encryption, compression).
  3. **Session** – Manages sessions and dialogues between computers.
  4. **Transport** – Reliable delivery of data (e.g., TCP).
  5. **Network** – Handles routing and logical addressing (e.g., IP).
  6. **Data Link** – Physical addressing and error detection (e.g., MAC address).
  7. **Physical** – Transmission of raw bits (e.g., cables, switches).

### 🖥️ TCP/IP Model
- Practical model used in real-world networks (like the internet).
- **Layers (Top to Bottom):**
  1. **Application** – Includes OSI's Application, Presentation, and Session layers.
  2. **Transport** – Same as OSI's transport; TCP/UDP reside here.
  3. **Internet** – Same as OSI’s Network layer; IP resides here.
  4. **Network Access (Link)** – Combines Data Link and Physical layers of OSI.

> 📌 **Mapping OSI to TCP/IP:**  
OSI 7 Layers → TCP/IP 4 Layers  
Application + Presentation + Session → Application  
Transport → Transport  
Network → Internet  
Data Link + Physical → Network Access

---

## 2. HTTP vs HTTPS, DNS, REST APIs

### 🔗 HTTP vs HTTPS

| Feature         | HTTP                             | HTTPS                          |
|----------------|----------------------------------|--------------------------------|
| Protocol        | HyperText Transfer Protocol      | HTTP with Secure Socket Layer |
| Security        | Not secure (data in plain text) | Encrypted using TLS/SSL       |
| Port            | 80                              | 443                            |
| Use Case        | Non-sensitive content            | Banking, login, e-commerce     |
| Certificate     | Not required                     | SSL/TLS certificate required   |

- **HTTPS** encrypts data between the browser and server, preventing interception (MITM attacks).

### 🌍 DNS (Domain Name System)
- Converts **domain names** (e.g., `google.com`) into **IP addresses** (e.g., `142.250.183.206`).
- Works like a **phonebook** for the internet.
- **Types of DNS Records:**
  - `A` – Maps domain to IPv4 address.
  - `AAAA` – Maps domain to IPv6 address.
  - `CNAME` – Canonical name (aliases).
  - `MX` – Mail exchange (for email).
  - `NS` – Name server.

### 📡 REST APIs (Representational State Transfer)
- A standard architecture for designing **web services**.
- Based on **stateless communication** using HTTP.
- Common **HTTP Methods:**
  - `GET` – Read data
  - `POST` – Create new data
  - `PUT` – Update existing data
  - `DELETE` – Remove data

> REST APIs are commonly used for backend services, mobile apps, and frontend communication.

---

## 3. TCP vs UDP

| Feature         | TCP (Transmission Control Protocol) | UDP (User Datagram Protocol)      |
|----------------|-------------------------------------|-----------------------------------|
| Type            | Connection-oriented                 | Connectionless                    |
| Reliability     | Reliable (acknowledgments, retries) | Unreliable (no guarantee)         |
| Speed           | Slower                              | Faster                            |
| Use Case        | File transfers, Emails, Web         | Video streaming, VoIP, Gaming     |
| Header Size     | 20 bytes                            | 8 bytes                           |
| Ordering        | Maintains order                     | No ordering                       |

### TCP Characteristics:
- **3-Way Handshake:** SYN → SYN-ACK → ACK
- **Flow & Congestion Control**
- **Error Checking & Correction**

### UDP Characteristics:
- **No Handshake**
- **Low overhead**
- Suitable for real-time apps.

---

## 4. IP Addressing (IPv4, IPv6, Subnetting)

### 🌐 IPv4 (Internet Protocol Version 4)
- **32-bit address**, represented as four decimal octets (e.g., `192.168.1.1`)
- Total of **~4.3 billion addresses**.
- **Classes:**
  - A (1.0.0.0 – 126.255.255.255)
  - B (128.0.0.0 – 191.255.255.255)
  - C (192.0.0.0 – 223.255.255.255)
  - D (Multicast), E (Reserved)

### 🆕 IPv6
- **128-bit address**, e.g., `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- Supports **~340 undecillion addresses**.
- Solves IPv4 exhaustion problem.
- Simplifies header processing.

### 🧮 Subnetting
- Divides IP networks into **subnets** to improve routing and security.
- **Subnet Mask:** Determines the network and host part of an IP address.
  - Example: `255.255.255.0` (or `/24`) means first 24 bits are network bits.
- Helps in:
  - Efficient IP address allocation
  - Network segmentation
  - Reducing broadcast traffic

> 📌 CIDR (Classless Inter-Domain Routing) uses subnet masks in the form `/x` to denote number of network bits.

---
---
