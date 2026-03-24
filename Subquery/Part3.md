# 📘 SUBQUERIES – PART 3 (SELECT + Types + Correlated Deep Dive)
### 🔷 1. Big Orders using HAVING + Subquery (Final Logic)

👉 **Condition**:

- num_items > avg_items
- order_amount > avg_amount

✅ **Final Query (Concept)**

```sql
SELECT 
    order_item_order_id,
    COUNT(*) AS num_items,
    SUM(order_item_subtotal) AS order_amount
FROM order_items
GROUP BY order_item_order_id
HAVING 
    COUNT(*) > (subquery for avg_items)
    AND 
    SUM(order_item_subtotal) > (subquery for avg_amount);

```
🔥 **Execution Flow (VERY IMPORTANT)**

```sql
Subquery 1 → calculates average items
Subquery 2 → calculates average amount
Outer query → performs GROUP BY
HAVING → filters groups
```
👉 Internally, it becomes:
```sql
HAVING COUNT(*) > 2.99
AND SUM(...) > 597
```
💡 **Key Learning**

✔️ Always understand subqueries first
✔️ Do NOT read SQL top → bottom
✔️ Break into parts → easier

### 🔷 2. Premium Customers Problem

👉 **Definition**:

- Customers whose total orders > average orders

##### Step 1: Orders per customer

```sql
SELECT order_customer_id, COUNT(*) AS total_orders
FROM orders
GROUP BY order_customer_id;
```
##### Step 2: Average orders
```sql
SELECT AVG(total_orders)
FROM (
    -- above query
) x;
```
##### Step 3: Final Query
```sql
SELECT order_customer_id, COUNT(*) AS total_orders
FROM orders
GROUP BY order_customer_id
HAVING COUNT(*) > (
    SELECT AVG(total_orders)
    FROM (
        SELECT COUNT(*) AS total_orders
        FROM orders
        GROUP BY order_customer_id
    ) x
);
```
👉 **Result**:

✔️ Premium customers (above average activity)

### 🔷 3. Subquery in SELECT Clause ⭐

👉 Use Case:
- Add an extra column dynamically

✅ **Example**:
```sql
SELECT 
    *,
    (
        SELECT AVG(total_orders)
        FROM (
            SELECT COUNT(*) AS total_orders
            FROM orders
            GROUP BY order_customer_id
        ) x
    ) AS avg_orders
FROM orders;
```
👉 **Result**:
✔️ Each row shows:

**Original data**
- Plus average orders
### 🔷 4. Example: Above Average Salary
Step 1:
```sql
SELECT AVG(salary) FROM employees;
Step 2:
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary) FROM employees
);
```
👉 Result:
✔️ Employees earning above average

### 🔷 5. Types of Subqueries 🔥
1️⃣ Scalar Subquery

👉 Returns:

- Single value (1 row, 1 column)
✅ Example:
```sql
SELECT AVG(salary) FROM employees;
```
👉 Used with:

=, >, <
2️⃣ Multi-row / Multi-column Subquery

👉 Returns:

- Multiple rows and/or columns
✅ Example:
```sql
SELECT * FROM (
    SELECT order_id, COUNT(*) 
    FROM order_items 
    GROUP BY order_id
) x;
```
👉 Used in:

FROM
JOIN
3️⃣ Correlated Subquery ⭐

👉 Definition:
- Inner query depends on outer query

👉 Execution:
✔️ Runs for each row

### 🔷 6. Problem: Employees > Department Average

👉 Compare salary with department average, not overall

### 🔷 7. Approach 1: JOIN + Subquery
Step 1: Department averages
```sql
SELECT department_id, AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id;
Step 2: Join
SELECT e.name, e.salary, e.department_id, d.avg_salary
FROM employees e
JOIN (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
) d
ON e.department_id = d.department_id
WHERE e.salary > d.avg_salary;
```
👉 Result:
✔️ Employees earning more than their department average

### 🔷 8. Approach 2: Correlated Subquery ⭐
```sql
SELECT *
FROM employees e
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE department_id = e.department_id
);
```
🔥 Execution Flow (Important)

For each employee:

- Take employee’s department
- Calculate that department’s average salary
- Compare with employee salary

👉 Key Point:
✔️ Inner query depends on outer query → correlated

### 🔷 9. Key Differences
|Type	|Execution|
|--------|------------|
|Normal Subquery |	Runs once|
|Correlated Subquery	|Runs per row|

### 🔷 10. Where Subqueries Can Be Used

✅ SELECT
✅ FROM
✅ JOIN
✅ WHERE
✅ HAVING

### 🔷 11. Final Interview-Level Learnings 🚀

✅ Subqueries are very powerful for complex logic
✅ Always break problems into smaller steps
✅ HAVING is used for group filtering
✅ Correlated subqueries work row-by-row
✅ Multiple solutions always exist
