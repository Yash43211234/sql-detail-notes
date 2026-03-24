# 📘 SUBQUERIES – PART 1 (Complete Notes in English)
### 🔷 1. What is a Subquery?

#### 👉 Definition:
- A subquery is a query inside another query.

#### 👉 Structure:

- Outer Query → main query
- Inner Query (Subquery) → runs inside

✅ **Example**:

```sql
SELECT *
FROM customers
WHERE customer_id IN (
    SELECT order_customer_id FROM orders
);
```

👉 **Important Rules**:

✔️ Inner query executes first
✔️ Its result is used by the outer query

👉 **Simple line**:

*“First solve the inner query, then the outer query.”*

### 🔷 2. Problem Statement

👉 **Find**:
Customers who have not placed any order

👉 **Tables**:

- customers
- orders

### 🔷 3. Method 1: Subquery using NOT IN

💡 **Logic**:

- Get customer IDs from orders
- Remove those from customers

✅ **Query**:

```sql
SELECT *
FROM customers
WHERE customer_id NOT IN (
    SELECT order_customer_id FROM orders
);
```
⚙️ **Execution Flow**:

- Inner query → fetch customer IDs from orders
- Outer query → filter customers not in that list

👉 **Result**:

- ✔️ Customers who have no orders

### 🔷 4. Method 2: LEFT JOIN

 💡 Logic:
- Left table → customers
- Right table → orders
- Non-matching rows → NULL

✅ Query:
```sql
SELECT c.*
FROM customers c
LEFT JOIN orders o
ON c.customer_id = o.order_customer_id
WHERE o.order_customer_id IS NULL;
```
👉 **Result**:
✔️ Same result (customers with no orders)

### 🔷 5. Method 3: EXCEPT / MINUS
💡 **Logic**:

Customers − Orders

✅ **Query**:

```sql
SELECT customer_id FROM customers
EXCEPT
SELECT order_customer_id FROM orders;
```
👉 **Result**:
✔️ Customers present in customers but not in orders

##### ⚠️ Important Rule:

- Same number of columns
- Same datatype

### 🔷 6. Method 4: NOT EXISTS (Correlated Subquery)

✅ **Query**:

```sql
SELECT *
FROM customers c
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.order_customer_id = c.customer_id
);
```
##### 🔥 Correlated Subquery Concept

|Type	                |    Behavior|
|---------------------------|--------|
|Normal Subquery	        |    Runs once|
|Correlated Subquery	    |    Runs for each row|

👉 **Example**:

**If 1 million customers**
- → Inner query runs 1 million times

**⚠️ Can cause performance issues**

### 🔷 7. EXISTS vs NOT EXISTS
**Condition	Meaning**
- EXISTS	Returns true if subquery returns any row
- NOT EXISTS	Returns true if subquery returns no rows

👉 **Important**:

- SELECT 1
- SELECT *
- SELECT column

✔️ All are same
👉 Because only existence matters, not the data

### 🔷 8. Reverse Problem

👉 **Find**:
- Customers who have placed at least one order

**✔ Method 1: IN**
```sql
SELECT *
FROM customers
WHERE customer_id IN (
    SELECT order_customer_id FROM orders
);
```
**✔ Method 2: INNER JOIN**
```sql
SELECT DISTINCT c.customer_id
FROM customers c
INNER JOIN orders o
ON c.customer_id = o.order_customer_id;
```
*⚠️ Use DISTINCT to avoid duplicates*

**✔ Method 3: INTERSECT**
```sql
SELECT customer_id FROM customers
INTERSECT
SELECT order_customer_id FROM orders;
```
**✔ Method 4: EXISTS**
```sql
SELECT *
FROM customers c
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.order_customer_id = c.customer_id
);
```
### 🔷 9. Subquery in FROM Clause ⭐

👉 **Use case**:
- *When direct calculation is not possible*

🔸 **Example**: Average Order Items

**Step 1:** Create intermediate table
```sql
SELECT 
    order_item_order_id AS order_id,
    COUNT(*) AS number_of_items,
    SUM(order_item_subtotal) AS order_amount
FROM order_items
GROUP BY order_item_order_id;
```
👉 *Output*:
**| order_id | items | amount |**

**Step 2:** Calculate averages
```sql
SELECT 
    AVG(number_of_items),
    AVG(order_amount)
FROM (
    -- above query
) x;
```
👉 Important Rule:
✔️ Alias is mandatory (here: x)

### 🔷 10. Example 2: Average Orders per Customer
**Step 1:**
```sql
SELECT 
    order_customer_id,
    COUNT(*) AS total_orders
FROM orders
GROUP BY order_customer_id;
```
**Step 2:**

```sql
SELECT AVG(total_orders)
FROM (
    -- above query
) x;
```
👉 **Result**:
*✔️ Average orders per customer ≈ 5.5*

### 🔷 11. Important Learnings (Interview Level 🔥)

✅ Subquery = query inside query
✅ Inner query runs first
✅ Correlated subquery runs per row
✅ EXISTS checks only presence
✅ One problem → multiple solutions:

- _Subquery
- Join
- Set operations
- EXISTS_

### 🔷 12. Performance Insight (Senior Level 💡)

👉 Correlated subquery:
❌ Slow (row-by-row execution)

👉 Normal subquery:
✅ Faster

👉 Note:
✔️ DB optimizer may internally optimize queries

🔥 Final Mindset

👉 In SQL:
One problem = multiple solutions

👉 Best solution depends on:

- Readability
- Performance
- Use case

✅ Part 1 completed (English version)

If you want next:
👉 I’ll create Part 2 (Alias + Big Orders + JOIN vs HAVING deep explanation) in same clean format 🚀
