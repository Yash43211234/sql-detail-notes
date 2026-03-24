# 📘 SUBQUERIES – PART 2 (Alias + Big Orders + JOIN vs HAVING)
### 🔷 1. Important Rule: When is Alias Required?

👉 **Alias is REQUIRED when:**

**Subquery is used in:**
- FROM clause
- JOIN clause

👉 **Because**:
✔️ Subquery acts like a temporary table
✔️ Every table must have a name → so alias is needed

✅ **Example**

👉 (Alias Required)
```sql
SELECT *
FROM (
    SELECT * FROM orders
) x;
```

👉 Alias NOT required when:

**Subquery is used in:**
- WHERE
- HAVING

✅ **Example** (Alias NOT Required)

```sql
SELECT *
FROM orders
WHERE amount > (
    SELECT AVG(amount) FROM orders
);
```
### 🔷 2. Problem: Find "Big Orders"

**👉 Definition of Big Orders:**

**An order is “big” if:**

✔️ Number of items > average number of items
✔️ Order amount > average order amount

### 🔷 3. Step-by-Step Thinking

**We need 2 structures (tables):**

- 🔹 Table A → Order-level Data

👉 **Contains**:

- order_id
- number of items
- order amount

✅ **Query**:

```sql
SELECT 
    order_item_order_id AS order_id,
    COUNT(*) AS num_items,
    SUM(order_item_subtotal) AS order_amount
FROM order_items
GROUP BY order_item_order_id;
🔹 Table B → Average Values
```
👉 **Contains**:

- avg_items
- avg_amount

✅ **Query**:

```sql
SELECT 
    AVG(num_items) AS avg_items,
    AVG(order_amount) AS avg_amount
FROM (
    -- Table A query
) x;
```
### 🔷 4. Approach 1: Multiple Subqueries + JOIN

💡 **Idea**:

- Combine Table A and Table B
- Use JOIN to compare values

**✅ Query Structure:**

```sql
SELECT *
FROM 
    (Table A subquery) A
JOIN 
    (Table B subquery) B
ON 
    A.num_items > B.avg_items
    AND A.order_amount > B.avg_amount;

```
**🔥 Important Concept: CROSS JOIN Behavior**

👉 **Here**:

- No direct join key is used
- So effectively → CROSS JOIN

👉 **Meaning**:

- Every row of A joins with every row of B

✔️ But since B has only 1 row (averages)
- → It attaches to all rows of A

✅ Result:

✔️ Only those orders where:

- items > average
- amount > average
### 🔷 5. Query Reading Trick (VERY IMPORTANT)

❌ Wrong way:

- Reading SQL top → bottom

✔️ Correct way:

-  Understand Subquery A (as a table)
-  Understand Subquery B (as a table)
-  Then read outer query

👉 Think like:

- SELECT ...
- FROM A
- JOIN B
- ON condition;

### 🔷 6. ON vs WHERE
- - Clause	When it works
- - ON	During join
- - WHERE	After join

👉 In this case:

✔️ ON is better
→ Filtering happens early (better performance)

### 🔷 7. Where Subqueries are Used Here

In this example, subqueries are used in:

✅ FROM
✅ JOIN
✅ (Can also be used in WHERE if needed)

### 🔷 8. Approach 2: Without JOIN (Using HAVING) ⭐

👉 Instead of joining, directly filter grouped data

✅ Query:
```sql
SELECT 
    order_item_order_id,
    COUNT(*) AS num_items,
    SUM(order_item_subtotal) AS order_amount
FROM order_items
GROUP BY order_item_order_id
HAVING 
    COUNT(*) > (
        SELECT AVG(num_items)
        FROM (
            SELECT COUNT(*) AS num_items
            FROM order_items
            GROUP BY order_item_order_id
        ) x
    );

```
🔥 Key Concept
- Clause	Works On
- WHERE	Rows
- HAVING	Groups

👉 Here:
✔️ HAVING filters aggregated data

### 🔷 9. Important Insight

👉 Same problem → 2 approaches:

- Subquery + JOIN
- GROUP BY + HAVING
### 🔷 10. Core Learning (Very Important)

✅ Subquery can behave like a table
✅ Multiple subqueries can be used together
✅ Complex problems → break into steps
✅ HAVING is powerful for aggregate filtering

🔥 Final Summary (Quick Revision)
🔹 **Alias Rules:**
- Needed → FROM / JOIN
- Not needed → WHERE / HAVING
🔹 **Big Orders Condition:**
- Items > average
- Amount > average
🔹 **Two Approaches:**
- JOIN subqueries
- HAVING with subquery
🔹** Best Practice:**

👉 Always break problem into small parts
