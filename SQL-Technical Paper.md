
## **ACID**

## ** Introduction**

Databases are integral to modern computing across numerous domains, including finance, healthcare, e-commerce, telecommunications, and enterprise software. These systems routinely perform thousands to millions of operations per second, often from numerous concurrent users. Ensuring correctness in such an environment is non-trivial.

---

## ** ACID Overview**

The ACID model ensures:

| Property | Purpose |
|---------|---------|
| **Atomicity** | Guarantees all-or-nothing behavior of transactions |
| **Consistency** | Ensures transactions move the database from one valid state to another |
| **Isolation** | Prevents concurrent transactions from interfering |
| **Durability** | Guarantees that committed transactions persist even after system failure |

Together, these properties maintain data correctness even under heavy load, errors, or hardware interruptions.

---

## ** Atomicity**

### **Definition**
Atomicity ensures that a transaction is treated as a single, indivisible operation. If **any** component of the transaction fails, **the entire transaction is rolled back**, leaving the database unchanged.

## Example
```
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;

UPDATE accounts SET balance = balance + 100 WHERE id = 2;

COMMIT;
```

##  Consistency 

Consistency ensures that a transaction always brings the database from one valid state to another, obeying all integrity constraints.
# Mechanisms

Constraints: Primary key, foreign key, unique, check constraints.
Triggers: Automatic rules enforcing business logic.
Application logic: Custom checks within the transaction.
```
CREATE TABLE accounts (

  id INT PRIMARY KEY,
  
  balance INT CHECK (balance >= 0)
);
```
### Isolation

Isolation ensures that concurrent transactions do not interfere with each other. Each transaction should behave as if it’s running alone.
### Durability

Durability guarantees that once a transaction is committed, it remains permanent, even in the event of power failures or crashes.

# CAP Theorem 

## Introduction
The **CAP Theorem**, formulated by Eric Brewer, is fundamental in distributed database systems. It states that a **distributed system cannot simultaneously guarantee all three properties**:

1. **Consistency (C)** – Every read sees the most recent write.
2. **Availability (A)** – Every request receives a response, even if some nodes fail.
3. **Partition Tolerance (P)** – The system continues to function even if network partitions occur.

In SQL and relational databases, understanding CAP is critical when deploying **distributed SQL systems** (e.g., MySQL Cluster, CockroachDB, PostgreSQL with replication).

---

## CAP Properties 

###  Consistency
- In SQL, **consistency** ensures all **ACID constraints** are satisfied:
  - Primary keys, foreign keys
  - Unique constraints
  - Check constraints
  - Triggers and business rules
- In distributed SQL databases, **strong consistency** guarantees that all nodes return the same data for a query after a commit.

**Example (Consistency in SQL):**
```sql
CREATE TABLE accounts (
    id INT PRIMARY KEY,
    balance INT CHECK (balance >= 0)
);

BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

## JOINS
In SQL, **joins** are used to combine rows from two or more tables based on a related column. Joins are essential for retrieving meaningful data across multiple tables.

### RIGHT JOIN (RIGHT OUTER JOIN)
Returns all rows from the right 
table, and matching rows from the left table.
```
SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.common_column = table2.common_column;
```
### FULL JOIN (FULL OUTER JOIN)
Returns all rows from both tables.
```
SELECT columns
FROM table1
FULL OUTER JOIN table2
ON table1.common_column = table2.common_column;
```
### CROSS JOIN
Returns the Cartesian product of two tables.
```
SELECT columns
FROM table1
CROSS JOIN table2;
```
## SQL Aggregations and Filters
SQL provides powerful tools to **aggregate data** (like sum, average) and **filter rows** (using conditions) to extract meaningful insights from your tables.
| Function | Description |
|----------|-------------|
| `COUNT()` | Counts the number of rows |
| `SUM()`   | Adds numeric values |
| `AVG()`   | Calculates the average |
| `MIN()`   | Returns the minimum value |
| `MAX()`   | Returns the maximum value |

### Combining Aggregations and Filters
Find departments with more than 5 employees
and average salary > 50000:
```
SELECT department_id, COUNT(*) AS emp_count, AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 5 AND AVG(salary) > 50000;
```
# SQL Normalization

## **1. Introduction**
**Normalization** is the process of organizing a database to:
1. Reduce data redundancy
2. Avoid data anomalies (insert, update, delete anomalies)
3. Ensure data integrity

Normalization divides data into **multiple related tables** and defines relationships between them.

---

## **2. Normal Forms (NF)**
### Types of Normal Forms
1)First Normal Form (1NF)
2)Second Normal Form (2NF)
3)Third Normal Form (3NF)
###  First Normal Form (1NF)
- **Rule:** Each column must contain **atomic values** (no repeating groups or arrays).
- **Example (Not 1NF):**
```
Student Table:
+----+--------+-------------+
| ID | Name   | Subjects    |
+----+--------+-------------+
| 1  | Alice  | Math, Physics |
| 2  | Bob    | Chemistry    |
+----+--------+-------------+
```
# SQL Indexes

## ** Introduction**
An **index** in SQL is a **database object** that improves the speed of data retrieval operations.  
Think of it like an index in a book—it helps the database **find rows faster** without scanning the entire table.

---

## **Benefits of Indexes**
1. Speeds up **SELECT** queries with `WHERE`, `JOIN`, `ORDER BY`, or `GROUP BY`.
2. Reduces **full table scans** for large tables.
3. Helps enforce **uniqueness** using **unique indexes**.

---

## **Types of Indexes**

### Single-Column Index
- Created on a single column.
**Example
```sql
CREATE INDEX idx_employee_name
ON employees(name);
```
### Unique Index
Ensures that all values in the indexed column(s) are unique.
```
CREATE UNIQUE INDEX idx_unique_email
ON employees(email);
```
### Full-Text Index
Supports text searching inside string columns (like searching words in documents).
```
CREATE FULLTEXT INDEX idx_content
ON articles(content);
```

# SQL Transactions
A **transaction** is a **sequence of one or more SQL operations** executed as a single unit of work.  
Transactions ensure **data integrity** and follow the **ACID properties**
#### Transaction Commands

BEGIN / START TRANSACTION – Start a transaction:
BEGIN TRANSACTION; or START TRANSACTION;

#### COMMIT 
Make all changes permanent:
COMMIT;

#### ROLLBACK 
Undo all changes in the transaction:
ROLLBACK;

#### SAVEPOINT 
Set a point to rollback partially:
SAVEPOINT savepoint_name;
ROLLBACK TO savepoint_name;

# SQL Locking Mechanisms
A lock is used to control access to data when multiple users are working with the database at the same time.
#### Types of Locks
#### Shared Lock (S)
Lets many users read the same data at the same time.
No one can write while a shared lock is held.
#### Exclusive Lock (X)
Lets one user write the data.
No one else can read or write until the lock is released.
#### Locking Levels
Row-level: Locks a single row. Most flexible.
Table-level: Locks the whole table. Blocks other users from accessing it.

## SQL Database Isolation Levels

### 1. What is Isolation?
Isolation controls **how transactions see changes made by other transactions**.  
Higher isolation reduces problems like dirty reads but may reduce performance.

---

### 2. Isolation Levels

### 2.1 Read Uncommitted
- Transactions can read **uncommitted changes** from others.  
- May see **dirty data**.  
- Fastest but least safe.

### 2.2 Read Committed
- Transactions can read only **committed data**.  
- Prevents **dirty reads**.  
- Non-repeatable reads are still possible.

### 2.3 Repeatable Read
- Ensures that if a transaction reads a row twice, the data **does not change** between reads.  
- Prevents **dirty reads** and **non-repeatable reads**.  
- Phantom rows may still appear.

### 2.4 Serializable
- Strictest level.  
- Transactions execute **as if one at a time**.  
- Prevents dirty reads, non-repeatable reads, and phantom rows.  
- Safest but slower.

---

## 3. Example
```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
BEGIN TRANSACTION;
-- SQL operations here
COMMIT;
# SQL Triggers
```

##  What is a Trigger?
A **trigger** is a special type of stored procedure that **automatically runs** when a specific event occurs in the database.  
Triggers are used to enforce **business rules, maintain audit logs, or validate data**.

---

##  Types of Triggers

###  Based on Timing
- **BEFORE Trigger** – Executes **before** an insert, update, or delete operation.  
- **AFTER Trigger** – Executes **after** an insert, update, or delete operation.
---

##  Example Triggers

### AFTER INSERT Trigger
```sql
CREATE TRIGGER trg_after_insert
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
    INSERT INTO audit_log(employee_id, action, action_time)
    VALUES (NEW.id, 'INSERT', NOW());
END;


