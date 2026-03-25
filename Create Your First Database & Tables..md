# 📘 SQL Course – Day 2 Notes

---

## 🔷 1. Introduction
- Day 2 focuses on:
  - Databases
  - Tables
  - Basic SQL operations
- Day 1 (Installation) was foundational but boring
- Now actual SQL learning begins 🚀

---

## 🔷 2. MySQL Setup Recap
- Installed:
  - MySQL Server (core engine)
  - MySQL Workbench (UI / IDE)

👉 You can use:
- Terminal (basic)
- Workbench (recommended UI)

---

## 🔷 3. What is a Database?
- Database = **Collection of tables**

👉 Example:
- Customers table
- Orders table
- Order Items table

---

## 🔷 4. What is a Table?
- Table = Data stored in:
  - Rows (records)
  - Columns (attributes)

👉 Similar to Excel sheet

### Example:
| customer_id | first_name | city |
|------------|-----------|------|
| 1          | John      | Delhi |
| 2          | Mary      | Mumbai |

- Each row = one entity (customer)
- Each column = attribute (name, city, etc.)

---

## 🔷 5. Real-World Example (E-commerce)
### 📝 Sign Up Flow:
- User fills:
  - Name
  - Email
  - Phone
  - Address

👉 Stored in:
- `customers` table

### 🔐 Sign In Flow:
- Data retrieved from database

### ✏️ Update Profile:
- Updates existing row in table

---

## 🔷 6. Types of Databases

### 🟢 Relational Databases (SQL-based)
- Tables are **connected (relations)**
- Use SQL language

**Examples:**
- MySQL
- PostgreSQL
- SQL Server
- SQLite
- MariaDB

---

### 🔴 NoSQL Databases
- No fixed relations
- Flexible structure
- Different query languages

**Examples:**
- MongoDB
- Cassandra
- HBase
- DynamoDB

👉 This course focuses on:
➡️ **Relational Databases**

---

## 🔷 7. Why "Relational"?
- Tables are connected via **common columns**

### Example:
- Customers Table → customer_id
- Orders Table → customer_id

👉 Relation:
- Which customer placed which order

---

## 🔷 8. ER Diagram (Entity Relationship)
- Shows:
  - Tables
  - Columns
  - Relationships

👉 Use Google:
```
ER diagram for retail / healthcare / banking
```

---

## 🔷 9. MySQL vs SQL

| MySQL | SQL |
|------|-----|
| Database system | Language |
| Stores data | Queries data |
| Engine | Communication tool |

👉 SQL is used to interact with MySQL

---

## 🔷 10. Basic SQL Commands

---

### 📌 Show Databases
```sql
SHOW DATABASES;
```

---

### 📌 Create Database
```sql
CREATE DATABASE retail_db;
```

---

### 📌 Use Database
```sql
USE retail_db;
```

---

### 📌 Check Current Database
```sql
SELECT DATABASE();
```

---

## 🔷 11. Create Table

### Example: Orders Table
```sql
CREATE TABLE orders (
    order_id INT,
    order_date DATETIME,
    customer_id INT,
    order_status VARCHAR(30)
);
```

---

### 📌 Show Tables
```sql
SHOW TABLES;
```

---

## 🔷 12. Data Types

| Data Type | Description |
|----------|------------|
| INT | Integer values (1, 2, 3) |
| VARCHAR(n) | String (text) |
| DATE | Only date |
| DATETIME | Date + time |
| FLOAT | Decimal values |

---

## 🔷 13. Insert Data

```sql
INSERT INTO orders VALUES
(1, '2023-10-10 10:00:00', 11599, 'closed');
```

---

### Insert Another Record
```sql
INSERT INTO orders VALUES
(2, '2023-10-11', 25600, 'pending');
```

---

## 🔷 14. View Data

```sql
SELECT * FROM orders;
```

---

## 🔷 15. Important Notes
- INT → no quotes
- VARCHAR / DATE → use quotes `' '`
- DATETIME can accept only DATE (time defaults to 00:00:00)

---

## 🔷 16. Comments in SQL

### Single Line:
```sql
-- This is a comment
# This is also a comment
```

### Multi-line:
```sql
/* This is
   multi-line
   comment */
```

---

## 🔷 17. Creating Customers Table (Assignment Idea)

```sql
CREATE TABLE customers (
    customer_id INT,
    first_name VARCHAR(30),
    last_name VARCHAR(30),
    email VARCHAR(50),
    phone VARCHAR(20),
    address VARCHAR(100),
    city VARCHAR(30),
    state VARCHAR(30),
    pincode INT
);
```

---

## 🔷 18. Order Items Table Concept
- One order → multiple items

### Example:
| order_id | product | quantity | price |
|---------|--------|---------|------|
| 2       | Bat    | 1       | 500 |
| 2       | Ball   | 5       | 100 |

👉 Use:
- `FLOAT` for price

---

## 🔷 19. Schema Concept (Important)

### In Some Databases:
```
Database → Schema → Tables
```

Example:
- retail_db
  - dev schema
  - prod schema

### In MySQL:
```
Database → Tables
```

👉 No schema layer

---

## 🔷 20. Drop Table

```sql
DROP TABLE orders;
```

---

## 🔷 21. Drop Database

```sql
DROP DATABASE retail_db;
```

---

## 🔷 22. Key Learnings
- Database = collection of tables
- Table = rows + columns
- SQL = language to interact
- Learned:
  - CREATE
  - INSERT
  - SELECT
  - DROP
- Understood relational concept

---

## 🔷 23. Assignment
- Create:
  - `customers` table (5 records)
  - `order_items` table (5 records)

---

## 🔥 Final Summary
👉 You now understand:
- Database basics
- Table creation
- Data insertion
- Real-world mapping

👉 Ready for next level 🚀
