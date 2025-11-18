# ACID Properties in Database Systems  
### A Technical Paper

## Table of Contents
1. Introduction  
2. What Are Transactions?  
3. The ACID Properties  
   - 3.1 Atomicity  
   - 3.2 Consistency  
   - 3.3 Isolation  
   - 3.4 Durability  
4. Real-World Implementation of ACID  
5. ACID vs. BASE  
6. Why ACID Matters  
7. Conclusion  

---

# 1. Introduction

Data integrity and reliability are fundamental to any application that processes information. Whether handling financial transactions, e-commerce operations, or healthcare data, systems must ensure **correctness**, **stability**, and **fault tolerance**.

Relational Database Management Systems (RDBMS) guarantee such reliability using **transactions**. To standardize behavior, a set of properties known as **ACID**—**Atomicity, Consistency, Isolation, Durability**—defines how transactions should function.

This paper explores ACID in depth, focusing on definitions, examples, and practical implementation techniques used in modern databases.

---

# 2. What Are Transactions?

A **transaction** is a sequence of operations performed as a single logical unit of work. Transactions ensure that a batch of SQL statements behaves predictably.

### Characteristics of a Transaction
- **Begin**: A transaction starts explicitly or implicitly.
- **Execute**: SQL operations (INSERT, UPDATE, DELETE, SELECT).
- **Commit**: Save all changes permanently.
- **Rollback**: Undo all operations within the transaction.

### Example Scenario
Consider transferring money between two accounts:

1. Deduct 100 USD from Account A.  
2. Add 100 USD to Account B.  

These steps must *either both succeed* or *both fail*. A failure in step 2 cannot leave step 1 committed. ACID ensures this reliability.

---

# 3. The ACID Properties

## 3.1 Atomicity

### Definition
**Atomicity** ensures that a transaction is treated as a single, indivisible unit.  
Either **all statements succeed**, or **none are applied**.

### Key Features
- No partial updates  
- Errors trigger automatic rollback  
- Protects from incomplete transactions due to:
  - Power loss  
  - System failures  
  - Application errors  

### Example
```sql
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 50 WHERE id = 1;
UPDATE accounts SET balance = balance + 50 WHERE id = 2;
COMMIT;
