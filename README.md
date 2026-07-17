# 📘 SQL Cheat Sheet

A quick reference guide for commonly used SQL commands.

---

# 1. Database Management

## Create Table

```sql
CREATE TABLE table_name (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);
```

## Add Column

```sql
ALTER TABLE table_name
ADD column_name INT;
```

## Drop Table

```sql
DROP TABLE table_name;
```

---

## Indexes

### Create Index

```sql
CREATE INDEX idx_name
ON table_name(column1);
```

### Drop Index

```sql
DROP INDEX idx_name;
```

---

## Views

### Create View

```sql
CREATE VIEW view_name AS
SELECT column1, column2
FROM table_name;
```

### Drop View

```sql
DROP VIEW view_name;
```

---

## Rename Table

```sql
RENAME TABLE old_table TO new_table;
```

---

# 2. Data Querying

## Select Data

### Select Columns

```sql
SELECT column1, column2
FROM table_name;
```

### Select All Columns

```sql
SELECT *
FROM table_name;
```

### Unique Values

```sql
SELECT DISTINCT column1
FROM table_name;
```

---

## Filtering

### WHERE

```sql
SELECT *
FROM table_name
WHERE column1 = 'value';
```

### IN

```sql
SELECT *
FROM table_name
WHERE column1 IN ('value1', 'value2');
```

### BETWEEN

```sql
SELECT *
FROM table_name
WHERE column1 BETWEEN 10 AND 50;
```

### LIKE

```sql
SELECT *
FROM table_name
WHERE column1 LIKE 'A%';
```

Common Wildcards

| Pattern | Meaning |
|---------|---------|
| `'A%'` | Starts with A |
| `'%A'` | Ends with A |
| `'%A%'` | Contains A |
| `'A_'` | A followed by one character |

---

## Sorting

### Ascending

```sql
SELECT *
FROM table_name
ORDER BY column1 ASC;
```

### Descending

```sql
SELECT *
FROM table_name
ORDER BY column1 DESC;
```

---

## Limit Results

```sql
SELECT *
FROM table_name
LIMIT 10;
```

---

# 3. Aggregate Functions

## COUNT

```sql
SELECT COUNT(*)
FROM table_name;
```

## SUM

```sql
SELECT SUM(column1)
FROM table_name;
```

## AVG

```sql
SELECT AVG(column1)
FROM table_name;
```

## MIN

```sql
SELECT MIN(column1)
FROM table_name;
```

## MAX

```sql
SELECT MAX(column1)
FROM table_name;
```

---

# 4. GROUP BY & HAVING

## GROUP BY

```sql
SELECT column1,
       COUNT(*)
FROM table_name
GROUP BY column1;
```

## HAVING

```sql
SELECT column1,
       COUNT(*)
FROM table_name
GROUP BY column1
HAVING COUNT(*) > 5;
```

---

# 5. Data Manipulation (DML)

## INSERT

```sql
INSERT INTO table_name(column1, column2)
VALUES ('value1', 'value2');
```

## UPDATE

```sql
UPDATE table_name
SET column1 = 'new_value'
WHERE id = 1;
```

## DELETE

```sql
DELETE FROM table_name
WHERE column1 = 'value';
```

## TRUNCATE

```sql
TRUNCATE TABLE table_name;
```

---

# 6. Joins

## INNER JOIN

```sql
SELECT *
FROM table1
INNER JOIN table2
ON table1.id = table2.id;
```

---

## LEFT JOIN

```sql
SELECT *
FROM table1
LEFT JOIN table2
ON table1.id = table2.id;
```

---

## RIGHT JOIN

```sql
SELECT *
FROM table1
RIGHT JOIN table2
ON table1.id = table2.id;
```

---

## FULL OUTER JOIN

```sql
SELECT *
FROM table1
FULL OUTER JOIN table2
ON table1.id = table2.id;
```

---

## CROSS JOIN

```sql
SELECT *
FROM table1
CROSS JOIN table2;
```

---

# 7. Set Operations

## UNION

```sql
SELECT column1
FROM table1

UNION

SELECT column1
FROM table2;
```

---

## UNION ALL

```sql
SELECT column1
FROM table1

UNION ALL

SELECT column1
FROM table2;
```

---

# 8. Window Functions

## ROW_NUMBER()

```sql
SELECT column1,
       ROW_NUMBER() OVER (
           PARTITION BY column2
           ORDER BY column3
       )
FROM table_name;
```

---

## RANK()

```sql
SELECT column1,
       RANK() OVER (
           PARTITION BY column2
           ORDER BY column3
       )
FROM table_name;
```

---

# 9. Subqueries

## Single Row Subquery

```sql
SELECT column1
FROM table1
WHERE column2 = (
    SELECT MAX(column2)
    FROM table2
);
```

---

# 10. Common Table Expressions (CTEs)

```sql
WITH CTE AS (
    SELECT column1,
           AVG(column2) AS avg_value
    FROM table_name
    GROUP BY column1
)

SELECT *
FROM CTE
WHERE avg_value > 50;
```

---

# 11. Transactions

## Commit Transaction

```sql
START TRANSACTION;

UPDATE table_name
SET column1 = 'value'
WHERE id = 1;

COMMIT;
```

---

## Rollback

```sql
ROLLBACK;
```

---

# 12. Stored Procedures

## Create Procedure

```sql
CREATE PROCEDURE GetEmployee(IN emp_id INT)
BEGIN
    SELECT *
    FROM employees
    WHERE id = emp_id;
END;
```

---

## Execute Procedure

```sql
CALL GetEmployee(1);
```

---

# 13. String Functions

## CONCAT

```sql
SELECT CONCAT(first_name, ' ', last_name)
FROM employees;
```

## SUBSTRING

```sql
SELECT SUBSTRING(column1, 1, 5)
FROM table_name;
```

---

# 14. Date Functions

## Current Date & Time

```sql
SELECT NOW();
```

---

# 15. Performance Optimization

## Create Index

```sql
CREATE INDEX idx_column
ON table_name(column1);
```

---

## Query Execution Plan

```sql
EXPLAIN
SELECT *
FROM table_name
WHERE column1 > 1000;
```

---

# 📚 SQL Learning Roadmap

- ✅ SELECT
- ✅ WHERE
- ✅ DISTINCT
- ✅ ORDER BY
- ✅ LIMIT
- ✅ Aggregate Functions
- ✅ GROUP BY
- ✅ HAVING
- ✅ CASE WHEN
- ✅ Joins
- ✅ Subqueries
- ✅ EXISTS / NOT EXISTS
- ✅ UNION / UNION ALL
- ✅ CTEs
- ✅ Window Functions
- ✅ Ranking Functions
- ✅ Transactions
- ✅ Stored Procedures
- ✅ Performance Optimization

---

## ⭐ If this cheat sheet helped you, consider giving the repository a star!
