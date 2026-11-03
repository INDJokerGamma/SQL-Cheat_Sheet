# 📘 Complete SQL Cheat Sheet

A comprehensive SQL quick reference covering basic to advanced SQL concepts, with syntax, practical queries, explanations, and examples. Useful for coding interviews, placement preparation, database development, and daily practice.

**Database:** MySQL  
**Level:** Beginner to Advanced

---

# 📑 Table of Contents

1. [SQL Basics](#1-sql-basics)
2. [Database Management](#2-database-management)
3. [Data Types and Constraints](#3-data-types-and-constraints)
4. [Data Querying - SELECT](#4-data-querying---select)
5. [Filtering and Operators](#5-filtering-and-operators)
6. [Sorting and Limiting](#6-sorting-and-limiting)
7. [Aggregate Functions](#7-aggregate-functions)
8. [GROUP BY and HAVING](#8-group-by-and-having)
9. [CASE WHEN](#9-case-when)
10. [Data Manipulation (DML)](#10-data-manipulation-dml)
11. [Joins](#11-joins)
12. [Set Operations](#12-set-operations)
13. [Subqueries](#13-subqueries)
14. [EXISTS and NOT EXISTS](#14-exists-and-not-exists)
15. [Common Table Expressions (CTEs)](#15-common-table-expressions-ctes)
16. [Window Functions](#16-window-functions)
17. [Ranking Functions](#17-ranking-functions)
18. [String Functions](#18-string-functions)
19. [Numeric Functions](#19-numeric-functions)
20. [Date and Time Functions](#20-date-and-time-functions)
21. [NULL Handling](#21-null-handling)
22. [Indexes](#22-indexes)
23. [Views](#23-views)
24. [Stored Procedures](#24-stored-procedures)
25. [Functions and Triggers](#25-functions-and-triggers)
26. [Transactions](#26-transactions)
27. [Performance Optimization](#27-performance-optimization)
28. [SQL Execution Order](#28-sql-execution-order)
29. [SQL Interview Quick Reference](#29-sql-interview-quick-reference)
30. [SQL Learning Roadmap](#30-sql-learning-roadmap)

---

# 1. SQL Basics

SQL stands for **Structured Query Language**. It is used to communicate with relational databases to store, retrieve, modify, and manage data.

### SQL Command Categories

| Category | Full Form | Purpose | Commands |
|---|---|---|---|
| DDL | Data Definition Language | Defines database structure | CREATE, ALTER, DROP, TRUNCATE |
| DML | Data Manipulation Language | Modifies table data | INSERT, UPDATE, DELETE |
| DQL | Data Query Language | Retrieves data | SELECT |
| DCL | Data Control Language | Controls permissions | GRANT, REVOKE |
| TCL | Transaction Control Language | Manages transactions | COMMIT, ROLLBACK, SAVEPOINT |

---

# 2. Database Management

### CREATE DATABASE

Creates a new database.

```sql
CREATE DATABASE company;
```

### SHOW DATABASES

Displays all available databases.

```sql
SHOW DATABASES;
```

### USE DATABASE

Selects a database to work with.

```sql
USE company;
```

### DROP DATABASE

Deletes an entire database and its contents.

```sql
DROP DATABASE company;
```

### CREATE TABLE

Creates a new table.

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    salary DECIMAL(10,2),
    department_id INT
);
```

### SHOW TABLES

Displays all tables in the selected database.

```sql
SHOW TABLES;
```

### DESCRIBE TABLE

Displays the structure of a table, including column names, data types, and constraints.

```sql
DESCRIBE employees;
```

### ALTER TABLE

Modifies an existing table structure.

**Add a column:**

```sql
ALTER TABLE employees
ADD email VARCHAR(100);
```

**Modify a column:**

```sql
ALTER TABLE employees
MODIFY salary DECIMAL(12,2);
```

**Rename a column:**

```sql
ALTER TABLE employees
RENAME COLUMN first_name TO employee_name;
```

**Drop a column:**

```sql
ALTER TABLE employees
DROP COLUMN email;
```

### RENAME TABLE

Changes the name of a table.

```sql
RENAME TABLE employees TO staff;
```

### DROP TABLE

Deletes the table structure and all its records.

```sql
DROP TABLE employees;
```

### TRUNCATE TABLE

Removes all records while keeping the table structure.

```sql
TRUNCATE TABLE employees;
```

**DROP vs TRUNCATE vs DELETE**

| Command | What it does |
|---|---|
| DELETE | Removes selected or all rows |
| TRUNCATE | Removes all rows but keeps the table |
| DROP | Removes the entire table |

---

# 3. Data Types and Constraints

## Common SQL Data Types

| Data Type | Meaning | Example |
|---|---|---|
| INT | Integer values | 100 |
| BIGINT | Large integer values | 1000000 |
| DECIMAL(p,s) | Exact decimal values | 12500.50 |
| FLOAT | Approximate decimal values | 12.5 |
| CHAR(n) | Fixed-length string | 'ABC' |
| VARCHAR(n) | Variable-length string | 'Piyush' |
| TEXT | Large text | Description |
| DATE | Date | 2026-11-03 |
| TIME | Time | 10:30:00 |
| DATETIME | Date and time | 2026-11-03 10:30:00 |
| BOOLEAN | True or false | TRUE |

## SQL Constraints

Constraints enforce rules on the data stored in a table.

### PRIMARY KEY

Uniquely identifies every row. It cannot contain NULL values or duplicates.

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    name VARCHAR(50)
);
```

### FOREIGN KEY

Creates a relationship between two tables by referencing a primary or unique key in another table.

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    student_id INT,
    FOREIGN KEY (student_id)
    REFERENCES students(student_id)
);
```

### UNIQUE

Ensures that values in a column are unique.

```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE
);
```

### NOT NULL

Prevents a column from storing NULL values.

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL
);
```

### CHECK

Ensures that values satisfy a specified condition.

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    salary DECIMAL(10,2) CHECK (salary > 0)
);
```

### DEFAULT

Assigns a default value when no value is provided.

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    country VARCHAR(50) DEFAULT 'India'
);
```

### AUTO_INCREMENT

Automatically generates sequential integer values for new rows in MySQL.

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50)
);
```

---

# 4. Data Querying - SELECT

SELECT is used to retrieve data from one or more tables.

### Select Specific Columns

```sql
SELECT first_name, salary
FROM employees;
```

**Meaning:** Retrieves only the first_name and salary columns.

### Select All Columns

```sql
SELECT *
FROM employees;
```

**Meaning:** Retrieves all columns from the table.

### DISTINCT

Returns only unique values by removing duplicate combinations of selected columns.

```sql
SELECT DISTINCT department_id
FROM employees;
```

**Meaning:** Displays each department only once.

### DISTINCT on Multiple Columns

```sql
SELECT DISTINCT department_id, job_id
FROM employees;
```

**Meaning:** Returns unique combinations of department_id and job_id.

### Column Aliases

Renames a column in the query output without changing the actual table structure.

```sql
SELECT
    first_name AS employee_name,
    salary AS monthly_salary
FROM employees;
```

### Calculated Columns

Performs calculations directly in the SELECT statement.

```sql
SELECT
    first_name,
    salary,
    salary * 12 AS annual_salary
FROM employees;
```

### CONCAT with SELECT

Combines multiple columns into a single string.

```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM employees;
```

---

# 5. Filtering and Operators

The WHERE clause filters rows based on specified conditions.

### WHERE

Returns only records satisfying a condition.

```sql
SELECT *
FROM employees
WHERE salary > 5000;
```

### Comparison Operators

| Operator | Meaning |
|---|---|
| = | Equal to |
| != or <> | Not equal to |
| > | Greater than |
| < | Less than |
| >= | Greater than or equal to |
| <= | Less than or equal to |

### AND

Both conditions must be true.

```sql
SELECT *
FROM employees
WHERE salary > 5000
AND department_id = 10;
```

### OR

At least one condition must be true.

```sql
SELECT *
FROM employees
WHERE department_id = 10
OR department_id = 20;
```

### NOT

Reverses a condition.

```sql
SELECT *
FROM employees
WHERE NOT department_id = 10;
```

### IN

Checks whether a value matches any value in a given list.

```sql
SELECT *
FROM employees
WHERE department_id IN (10, 20, 30);
```

**Meaning:** Returns employees belonging to departments 10, 20, or 30.

### NOT IN

Excludes specified values.

```sql
SELECT *
FROM employees
WHERE department_id NOT IN (10, 20);
```

### BETWEEN

Checks whether a value falls within an inclusive range.

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 5000 AND 10000;
```

**Meaning:** Includes salaries equal to 5000 and 10000.

### NOT BETWEEN

Excludes values within a specified range.

```sql
SELECT *
FROM employees
WHERE salary NOT BETWEEN 5000 AND 10000;
```

### LIKE

Searches for a specific pattern in a string.

```sql
SELECT *
FROM employees
WHERE first_name LIKE 'A%';
```

**Common Wildcards**

| Pattern | Meaning |
|---|---|
| 'A%' | Starts with A |
| '%A' | Ends with A |
| '%A%' | Contains A |
| 'A_' | A followed by exactly one character |
| '_A%' | Second character is A |
| 'A%B' | Starts with A and ends with B |

### NOT LIKE

Excludes values matching a specified pattern.

```sql
SELECT *
FROM employees
WHERE first_name NOT LIKE 'A%';
```

### IS NULL

Checks whether a column contains NULL.

```sql
SELECT *
FROM employees
WHERE manager_id IS NULL;
```

### IS NOT NULL

Returns records where a column has a non-NULL value.

```sql
SELECT *
FROM employees
WHERE manager_id IS NOT NULL;
```

---

# 6. Sorting and Limiting

### ORDER BY ASC

Sorts results in ascending order.

```sql
SELECT *
FROM employees
ORDER BY salary ASC;
```

**Meaning:** Lowest salary appears first.

### ORDER BY DESC

Sorts results in descending order.

```sql
SELECT *
FROM employees
ORDER BY salary DESC;
```

**Meaning:** Highest salary appears first.

### Multiple Column Sorting

Sorts by one column first and uses another column to break ties.

```sql
SELECT *
FROM employees
ORDER BY department_id ASC, salary DESC;
```

### LIMIT

Restricts the number of returned rows.

```sql
SELECT *
FROM employees
LIMIT 5;
```

**Meaning:** Returns only the first five rows.

### LIMIT with OFFSET

Skips a specified number of rows before returning results.

```sql
SELECT *
FROM employees
LIMIT 5 OFFSET 10;
```

**Meaning:** Skips the first 10 rows and returns the next 5.

### Find the Highest Salary

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 1;
```

### Find the Second Highest Salary

```sql
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
```

---

# 7. Aggregate Functions

Aggregate functions perform calculations on multiple rows and return a single result.

### COUNT()

Counts the number of rows.

```sql
SELECT COUNT(*)
FROM employees;
```

**COUNT(*) vs COUNT(column)**

- COUNT(*) counts all rows.
- COUNT(column) counts only non-NULL values in that column.
- COUNT(DISTINCT column) counts unique non-NULL values.

```sql
SELECT COUNT(DISTINCT department_id)
FROM employees;
```

### SUM()

Returns the sum of numeric values.

```sql
SELECT SUM(salary)
FROM employees;
```

### AVG()

Calculates the average of non-NULL numeric values.

```sql
SELECT AVG(salary)
FROM employees;
```

### MIN()

Returns the smallest value.

```sql
SELECT MIN(salary)
FROM employees;
```

### MAX()

Returns the largest value.

```sql
SELECT MAX(salary)
FROM employees;
```

### Multiple Aggregate Functions

```sql
SELECT
    COUNT(*) AS total_employees,
    SUM(salary) AS total_salary,
    AVG(salary) AS average_salary,
    MIN(salary) AS minimum_salary,
    MAX(salary) AS maximum_salary
FROM employees;
```

---

# 8. GROUP BY and HAVING

## GROUP BY

Groups rows having the same values in specified columns.

```sql
SELECT department_id, COUNT(*) AS total_employees
FROM employees
GROUP BY department_id;
```

**Meaning:** Creates one group per department and counts the employees in each group.

### Multiple Column GROUP BY

```sql
SELECT department_id, job_id, COUNT(*)
FROM employees
GROUP BY department_id, job_id;
```

**Meaning:** Groups records using unique combinations of department_id and job_id.

## HAVING

Filters groups after GROUP BY is performed.

```sql
SELECT department_id, AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 8000;
```

**Meaning:** Displays only departments whose average salary exceeds 8000.

## WHERE vs HAVING

| WHERE | HAVING |
|---|---|
| Filters individual rows | Filters grouped results |
| Applied before GROUP BY | Applied after GROUP BY |
| Generally used without aggregate conditions | Commonly used with aggregate conditions |
| Cannot directly filter aggregate results | Can filter aggregate results |

### WHERE and HAVING Together

```sql
SELECT department_id, AVG(salary)
FROM employees
WHERE salary > 3000
GROUP BY department_id
HAVING AVG(salary) > 8000;
```

**Execution:**
1. WHERE removes employees earning 3000 or less.
2. GROUP BY groups remaining employees by department.
3. AVG calculates the average salary.
4. HAVING retains departments with average salary above 8000.

---

# 9. CASE WHEN

CASE WHEN provides conditional logic in SQL, similar to if-else statements in programming languages.

### Basic CASE WHEN

```sql
SELECT
    first_name,
    salary,
    CASE
        WHEN salary >= 10000 THEN 'High'
        WHEN salary >= 5000 THEN 'Medium'
        ELSE 'Low'
    END AS salary_category
FROM employees;
```

### Multiple Conditions

```sql
SELECT
    first_name,
    CASE
        WHEN salary > 10000 AND department_id = 10 THEN 'Category A'
        WHEN salary > 5000 AND department_id = 20 THEN 'Category B'
        ELSE 'Other'
    END AS category
FROM employees;
```

### CASE WHEN with GROUP BY

```sql
SELECT
    department_id,
    SUM(
        CASE
            WHEN salary > 5000 THEN 1
            ELSE 0
        END
    ) AS high_salary_count
FROM employees
GROUP BY department_id;
```

**Meaning:** Counts employees earning more than 5000 in each department.

### Conditional Aggregation

```sql
SELECT
    COUNT(CASE WHEN salary > 5000 THEN 1 END) AS high_salary,
    COUNT(CASE WHEN salary <= 5000 THEN 1 END) AS low_salary
FROM employees;
```

---

# 10. Data Manipulation (DML)

## INSERT

Adds new records to a table.

### Insert One Record

```sql
INSERT INTO employees
(employee_id, first_name, salary)
VALUES (101, 'Piyush', 8000);
```

### Insert Multiple Records

```sql
INSERT INTO employees
(employee_id, first_name, salary)
VALUES
(102, 'Amit', 7000),
(103, 'Rahul', 9000);
```

### INSERT INTO SELECT

Copies records from one table into another.

```sql
INSERT INTO employee_backup
(employee_id, first_name, salary)
SELECT employee_id, first_name, salary
FROM employees;
```

## UPDATE

Modifies existing records.

```sql
UPDATE employees
SET salary = 10000
WHERE employee_id = 101;
```

### Update Multiple Columns

```sql
UPDATE employees
SET salary = 12000,
    department_id = 20
WHERE employee_id = 101;
```

**Important:** Always use WHERE when you intend to update specific records. Without WHERE, all rows are updated.

## DELETE

Removes selected records.

```sql
DELETE FROM employees
WHERE employee_id = 101;
```

### Delete All Records

```sql
DELETE FROM employees;
```

**Important:** DELETE without WHERE removes all rows but retains the table structure.

---

# 11. Joins

Joins combine records from two or more tables using a related column.

Assume:

**Table 1: employees**

| employee_id | name | department_id |
|---|---|---|
| 1 | Amit | 10 |
| 2 | Rahul | 20 |
| 3 | Piyush | 30 |
| 4 | Neha | NULL |

**Table 2: departments**

| department_id | department_name |
|---|---|
| 10 | HR |
| 20 | IT |
| 40 | Finance |

## 11.1 INNER JOIN

Returns only matching records from both tables.

```sql
SELECT *
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id;
```

**Meaning:**
- Takes matching rows from the first table.
- Takes matching rows from the second table.
- Rows without a match in either table are excluded.

**Result:** Amit and Rahul are returned. Piyush and Neha are excluded because their department IDs have no matching department record.

## 11.2 LEFT JOIN (LEFT OUTER JOIN)

Returns all records from the LEFT table and only matching records from the RIGHT table.

```sql
SELECT *
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id;
```

**Meaning:**
- Takes ALL rows from the first table (employees).
- Takes only matching rows from the second table (departments).
- If a row from the first table has no match, the second table's columns contain NULL.

**Result:** All four employees are returned. Piyush and Neha have NULL department details.

## 11.3 RIGHT JOIN (RIGHT OUTER JOIN)

Returns all records from the RIGHT table and only matching records from the LEFT table.

```sql
SELECT *
FROM employees e
RIGHT JOIN departments d
ON e.department_id = d.department_id;
```

**Meaning:**
- Takes ALL rows from the second table (departments).
- Takes only matching rows from the first table (employees).
- If a department has no matching employee, employee columns contain NULL.

**Result:** HR, IT, and Finance are returned. Finance has NULL employee details.

## 11.4 FULL OUTER JOIN

Returns all records from both tables, matching rows where possible.

```sql
SELECT *
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id

UNION

SELECT *
FROM employees e
RIGHT JOIN departments d
ON e.department_id = d.department_id;
```

**Meaning:**
- Returns all rows from the first table.
- Returns all rows from the second table.
- Matches related records.
- Unmatched columns are filled with NULL.

**Note:** MySQL does not directly support FULL OUTER JOIN. The above UNION approach simulates it.

## 11.5 CROSS JOIN

Returns every possible combination of rows from both tables.

```sql
SELECT *
FROM employees
CROSS JOIN departments;
```

**Meaning:** If the first table has 4 rows and the second table has 3 rows, the result contains 12 rows.

## 11.6 SELF JOIN

Joins a table with itself.

Useful when records in the same table are related, such as employees and their managers.

```sql
SELECT
    e.first_name AS employee_name,
    m.first_name AS manager_name
FROM employees e
JOIN employees m
ON e.manager_id = m.employee_id;
```

**Meaning:**
- `e` represents the employee.
- `m` represents the manager.
- Both aliases refer to the same employees table.

## 11.7 Multiple JOINs

Joins more than two tables in a single query.

```sql
SELECT
    e.first_name,
    d.department_name,
    l.city
FROM employees e
JOIN departments d
ON e.department_id = d.department_id
JOIN locations l
ON d.location_id = l.location_id;
```

**Meaning:** Combines employee, department, and location information.

## 11.8 JOIN with WHERE

Filters joined records.

```sql
SELECT
    e.first_name,
    d.department_name
FROM employees e
JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_name = 'IT';
```

## 11.9 LEFT JOIN with IS NULL

Finds records in the first table that have no matching record in the second table.

```sql
SELECT e.*
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

**Meaning:** Finds employees whose department does not exist in the departments table, including employees with a NULL department_id.

**Find departments without employees:**

```sql
SELECT d.*
FROM departments d
LEFT JOIN employees e
ON d.department_id = e.department_id
WHERE e.employee_id IS NULL;
```

### Join Quick Reference

| Join | First Table | Second Table | Result |
|---|---|---|---|
| INNER JOIN | Matching rows | Matching rows | Only common records |
| LEFT JOIN | All rows | Matching rows | All left records |
| RIGHT JOIN | Matching rows | All rows | All right records |
| FULL OUTER JOIN | All rows | All rows | Everything from both |
| CROSS JOIN | All rows | All rows | Every possible combination |
| SELF JOIN | Same table | Same table | Related records within one table |

---

# 12. Set Operations

Set operations combine the results of multiple SELECT statements.

The SELECT statements must return the same number of columns with compatible data types.

## UNION

Combines results and removes duplicate rows.

```sql
SELECT department_id
FROM employees

UNION

SELECT department_id
FROM departments;
```

**Meaning:** Combines both results and keeps only unique values.

## UNION ALL

Combines results without removing duplicates.

```sql
SELECT department_id
FROM employees

UNION ALL

SELECT department_id
FROM departments;
```

**Meaning:** Returns all rows, including duplicates.

## INTERSECT

Returns rows common to both query results.

```sql
SELECT department_id
FROM employees

INTERSECT

SELECT department_id
FROM departments;
```

**Note:** Supported in MySQL 8.0.31 and later.

## EXCEPT

Returns rows present in the first query but absent from the second query.

```sql
SELECT department_id
FROM departments

EXCEPT

SELECT department_id
FROM employees;
```

**Note:** Supported in MySQL 8.0.31 and later.

### UNION vs UNION ALL

| UNION | UNION ALL |
|---|---|
| Removes duplicates | Keeps duplicates |
| Performs duplicate elimination | Does not perform duplicate elimination |
| Usually slower | Usually faster |

---

# 13. Subqueries

A subquery is a query written inside another SQL query.

It is used when the result of one query is needed by another query.

## 13.1 Single Row Subquery

Returns one row and one value.

```sql
SELECT first_name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

**Meaning:** Finds employees earning more than the company's average salary.

## 13.2 Multiple Row Subquery

Returns multiple rows. Commonly used with IN, ANY, and ALL.

```sql
SELECT first_name
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM departments
    WHERE location_id = 1700
);
```

**Meaning:** Finds employees working in departments located at location 1700.

## 13.3 Scalar Subquery

Returns a single value that can be used in a SELECT list or expression.

```sql
SELECT
    first_name,
    salary,
    (SELECT AVG(salary) FROM employees) AS company_avg_salary
FROM employees;
```

## 13.4 Multiple Column Subquery

Returns multiple columns.

```sql
SELECT *
FROM employees
WHERE (department_id, job_id) IN (
    SELECT department_id, job_id
    FROM employees
    WHERE salary > 10000
);
```

## 13.5 Correlated Subquery

A correlated subquery depends on the outer query and executes logically for each outer row.

```sql
SELECT e.first_name, e.salary, e.department_id
FROM employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

**Meaning:** Finds employees earning more than the average salary of their own department.

## 13.6 Subquery with FROM

Uses a subquery as a temporary derived table.

```sql
SELECT *
FROM (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
) AS dept_summary
WHERE avg_salary > 8000;
```

## 13.7 ANY

Compares a value with at least one value returned by a subquery.

```sql
SELECT *
FROM employees
WHERE salary > ANY (
    SELECT salary
    FROM employees
    WHERE department_id = 20
);
```

**Meaning:** Returns employees whose salary is greater than at least one salary in department 20.

## 13.8 ALL

Compares a value with every value returned by a subquery.

```sql
SELECT *
FROM employees
WHERE salary > ALL (
    SELECT salary
    FROM employees
    WHERE department_id = 20
);
```

**Meaning:** Returns employees whose salary is greater than every salary in department 20.

### ANY vs ALL

| ANY | ALL |
|---|---|
| At least one condition must be true | Every comparison must be true |
| Similar to OR | Similar to AND |

---

# 14. EXISTS and NOT EXISTS

## EXISTS

Checks whether a subquery returns at least one row.

```sql
SELECT *
FROM departments d
WHERE EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

**Meaning:** Returns departments that have at least one employee.

## NOT EXISTS

Checks whether a subquery returns no rows.

```sql
SELECT *
FROM departments d
WHERE NOT EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

**Meaning:** Returns departments that do not have any employees.

### EXISTS vs IN

| EXISTS | IN |
|---|---|
| Checks whether matching rows exist | Checks whether a value belongs to a returned list |
| Commonly used with correlated subqueries | Commonly used with single-column subqueries |
| Can stop searching after finding a match | Compares against the subquery result |
| Handles NULL-related situations differently from NOT IN | NOT IN can behave unexpectedly when its list contains NULL |

---

# 15. Common Table Expressions (CTEs)

A CTE creates a temporary named result set that can be referenced within a single SQL statement.

## Basic CTE

```sql
WITH dept_summary AS (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)

SELECT *
FROM dept_summary
WHERE avg_salary > 8000;
```

**Meaning:** Calculates average salary per department and then filters the result.

## Multiple CTEs

```sql
WITH
dept_avg AS (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
),
dept_count AS (
    SELECT department_id, COUNT(*) AS total_employees
    FROM employees
    GROUP BY department_id
)

SELECT
    a.department_id,
    a.avg_salary,
    c.total_employees
FROM dept_avg a
JOIN dept_count c
ON a.department_id = c.department_id;
```

## Recursive CTE

A recursive CTE references itself to process hierarchical or sequential data.

```sql
WITH RECURSIVE numbers AS (
    SELECT 1 AS n

    UNION ALL

    SELECT n + 1
    FROM numbers
    WHERE n < 10
)

SELECT *
FROM numbers;
```

**Meaning:** Generates numbers from 1 to 10.

---

# 16. Window Functions

Window functions perform calculations across related rows without collapsing them into a single row like GROUP BY.

## OVER()

Defines the window of rows used by a window function.

```sql
SELECT
    first_name,
    salary,
    AVG(salary) OVER() AS company_avg
FROM employees;
```

**Meaning:** Displays every employee along with the company's average salary.

## PARTITION BY

Divides rows into groups for window calculations without removing individual rows.

```sql
SELECT
    first_name,
    department_id,
    salary,
    AVG(salary) OVER(
        PARTITION BY department_id
    ) AS dept_avg_salary
FROM employees;
```

**Meaning:** Calculates the average salary separately for each department.

## ORDER BY inside OVER()

Defines the order in which rows are processed within the window.

```sql
SELECT
    first_name,
    salary,
    SUM(salary) OVER(
        ORDER BY employee_id
    ) AS running_total
FROM employees;
```

**Meaning:** Calculates a running total of salaries.

## Running Average

```sql
SELECT
    first_name,
    salary,
    AVG(salary) OVER(
        ORDER BY employee_id
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS running_avg
FROM employees;
```

## LAG()

Accesses a value from a previous row.

```sql
SELECT
    first_name,
    salary,
    LAG(salary) OVER(
        ORDER BY employee_id
    ) AS previous_salary
FROM employees;
```

**Meaning:** Displays the salary of the previous employee in the specified ordering.

## LEAD()

Accesses a value from a following row.

```sql
SELECT
    first_name,
    salary,
    LEAD(salary) OVER(
        ORDER BY employee_id
    ) AS next_salary
FROM employees;
```

## FIRST_VALUE()

Returns the first value in a window.

```sql
SELECT
    first_name,
    salary,
    FIRST_VALUE(salary) OVER(
        PARTITION BY department_id
        ORDER BY salary DESC
    ) AS highest_salary
FROM employees;
```

## LAST_VALUE()

Returns the last value in the defined window frame.

```sql
SELECT
    first_name,
    salary,
    LAST_VALUE(salary) OVER(
        PARTITION BY department_id
        ORDER BY salary
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND UNBOUNDED FOLLOWING
    ) AS highest_salary
FROM employees;
```

**Note:** LAST_VALUE depends on the window frame. Without an appropriate frame, it may return the current row's value rather than the final value in the partition.

---

# 17. Ranking Functions

Ranking functions assign a rank to rows based on specified ordering.

## ROW_NUMBER()

Assigns a unique sequential number to each row.

```sql
SELECT
    first_name,
    salary,
    ROW_NUMBER() OVER(
        ORDER BY salary DESC
    ) AS row_num
FROM employees;
```

**Meaning:** Assigns 1, 2, 3, 4... even when salaries are tied.

## RANK()

Assigns the same rank to tied values and skips subsequent rank numbers.

```sql
SELECT
    first_name,
    salary,
    RANK() OVER(
        ORDER BY salary DESC
    ) AS salary_rank
FROM employees;
```

**Example:** 1, 2, 2, 4.

## DENSE_RANK()

Assigns the same rank to tied values without skipping subsequent rank numbers.

```sql
SELECT
    first_name,
    salary,
    DENSE_RANK() OVER(
        ORDER BY salary DESC
    ) AS salary_rank
FROM employees;
```

**Example:** 1, 2, 2, 3.

## NTILE()

Divides ordered rows into approximately equal groups.

```sql
SELECT
    first_name,
    salary,
    NTILE(4) OVER(
        ORDER BY salary DESC
    ) AS salary_group
FROM employees;
```

**Meaning:** Divides employees into four salary groups.

## Ranking Comparison

| Function | Ties | Skips Rank |
|---|---|---|
| ROW_NUMBER() | Unique numbers | No |
| RANK() | Same rank | Yes |
| DENSE_RANK() | Same rank | No |
| NTILE(n) | Divides into groups | Not a ranking function in the traditional sense |

## Find the Second Highest Salary Using DENSE_RANK

```sql
WITH ranked_salary AS (
    SELECT
        first_name,
        salary,
        DENSE_RANK() OVER(
            ORDER BY salary DESC
        ) AS rnk
    FROM employees
)

SELECT *
FROM ranked_salary
WHERE rnk = 2;
```

## Find Top 3 Salaries in Each Department

```sql
WITH ranked_employees AS (
    SELECT
        first_name,
        salary,
        department_id,
        DENSE_RANK() OVER(
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rnk
    FROM employees
)

SELECT *
FROM ranked_employees
WHERE rnk <= 3;
```

---

# 18. String Functions

String functions manipulate text values.

### CONCAT()

Combines two or more strings.

```sql
SELECT CONCAT(first_name, ' ', last_name)
FROM employees;
```

### LENGTH()

Returns the length of a string in bytes in MySQL.

```sql
SELECT LENGTH(first_name)
FROM employees;
```

### CHAR_LENGTH()

Returns the number of characters in a string.

```sql
SELECT CHAR_LENGTH(first_name)
FROM employees;
```

### UPPER()

Converts text to uppercase.

```sql
SELECT UPPER(first_name)
FROM employees;
```

### LOWER()

Converts text to lowercase.

```sql
SELECT LOWER(first_name)
FROM employees;
```

### SUBSTRING()

Extracts part of a string.

```sql
SELECT SUBSTRING(first_name, 1, 3)
FROM employees;
```

**Meaning:** Returns the first three characters.

### LEFT()

Extracts characters from the left side.

```sql
SELECT LEFT(first_name, 3)
FROM employees;
```

### RIGHT()

Extracts characters from the right side.

```sql
SELECT RIGHT(first_name, 3)
FROM employees;
```

### TRIM()

Removes leading and trailing spaces.

```sql
SELECT TRIM('  Piyush  ');
```

### LTRIM()

Removes leading spaces.

```sql
SELECT LTRIM('  Piyush');
```

### RTRIM()

Removes trailing spaces.

```sql
SELECT RTRIM('Piyush  ');
```

### REPLACE()

Replaces occurrences of a substring.

```sql
SELECT REPLACE('Hello World', 'World', 'SQL');
```

### INSTR()

Returns the position of a substring within a string.

```sql
SELECT INSTR('Hello World', 'World');
```

### REVERSE()

Reverses a string.

```sql
SELECT REVERSE('SQL');
```

### LPAD() and RPAD()

Adds characters to the left or right until a specified length is reached.

```sql
SELECT LPAD('123', 5, '0');
SELECT RPAD('123', 5, '0');
```

---

# 19. Numeric Functions

### ABS()

Returns the absolute value.

```sql
SELECT ABS(-100);
```

### ROUND()

Rounds a number to a specified number of decimal places.

```sql
SELECT ROUND(123.456, 2);
```

### CEIL()

Returns the smallest integer greater than or equal to a number.

```sql
SELECT CEIL(12.3);
```

### FLOOR()

Returns the largest integer less than or equal to a number.

```sql
SELECT FLOOR(12.9);
```

### MOD()

Returns the remainder after division.

```sql
SELECT MOD(10, 3);
```

### POWER()

Raises a number to a specified power.

```sql
SELECT POWER(2, 3);
```

### SQRT()

Returns the square root.

```sql
SELECT SQRT(25);
```

---

# 20. Date and Time Functions

### NOW()

Returns the current date and time.

```sql
SELECT NOW();
```

### CURDATE()

Returns the current date.

```sql
SELECT CURDATE();
```

### CURTIME()

Returns the current time.

```sql
SELECT CURTIME();
```

### YEAR()

Extracts the year from a date.

```sql
SELECT YEAR(hire_date)
FROM employees;
```

### MONTH()

Extracts the month.

```sql
SELECT MONTH(hire_date)
FROM employees;
```

### DAY()

Extracts the day of the month.

```sql
SELECT DAY(hire_date)
FROM employees;
```

### DATEDIFF()

Returns the difference between two dates in days.

```sql
SELECT DATEDIFF(CURDATE(), hire_date)
FROM employees;
```

### DATE_ADD()

Adds an interval to a date.

```sql
SELECT DATE_ADD(CURDATE(), INTERVAL 30 DAY);
```

### DATE_SUB()

Subtracts an interval from a date.

```sql
SELECT DATE_SUB(CURDATE(), INTERVAL 1 MONTH);
```

### TIMESTAMPDIFF()

Calculates the difference between two date/time values in a specified unit.

```sql
SELECT TIMESTAMPDIFF(YEAR, hire_date, CURDATE())
FROM employees;
```

### DATE_FORMAT()

Formats a date into a string.

```sql
SELECT DATE_FORMAT(hire_date, '%d-%m-%Y')
FROM employees;
```

---

# 21. NULL Handling

NULL represents missing or unknown data. It is not equivalent to zero or an empty string.

### IFNULL()

Returns an alternative value if the expression is NULL.

```sql
SELECT IFNULL(commission, 0)
FROM employees;
```

### COALESCE()

Returns the first non-NULL expression.

```sql
SELECT COALESCE(commission, bonus, 0)
FROM employees;
```

### NULLIF()

Returns NULL if two expressions are equal; otherwise returns the first expression.

```sql
SELECT NULLIF(salary, 0)
FROM employees;
```

### IF()

Returns one value when a condition is true and another when false. MySQL-specific.

```sql
SELECT
    first_name,
    IF(salary > 5000, 'High', 'Low') AS category
FROM employees;
```

### Important NULL Rules

```sql
-- Incorrect
SELECT *
FROM employees
WHERE manager_id = NULL;

-- Correct
SELECT *
FROM employees
WHERE manager_id IS NULL;
```

**Important:** Comparisons with NULL using `=`, `!=`, or other normal comparison operators produce UNKNOWN rather than TRUE or FALSE.

---

# 22. Indexes

An index improves data retrieval speed by helping the database locate records efficiently.

### CREATE INDEX

```sql
CREATE INDEX idx_salary
ON employees(salary);
```

### UNIQUE INDEX

Prevents duplicate values in the indexed column or combination of columns.

```sql
CREATE UNIQUE INDEX idx_email
ON employees(email);
```

### Composite Index

Creates an index using multiple columns.

```sql
CREATE INDEX idx_dept_salary
ON employees(department_id, salary);
```

### DROP INDEX

Removes an existing index.

```sql
DROP INDEX idx_salary
ON employees;
```

### SHOW INDEX

Displays indexes defined on a table.

```sql
SHOW INDEX FROM employees;
```

### Types of Indexes

| Index | Meaning |
|---|---|
| Single-column | Index on one column |
| Composite | Index on multiple columns |
| Unique | Prevents duplicate indexed values |
| Primary | Index associated with a primary key |
| Full-text | Supports text searching |
| Spatial | Supports spatial data operations |

**Important:** Indexes improve many read operations but consume storage and can slow down INSERT, UPDATE, and DELETE operations because indexes also need maintenance.

---

# 23. Views

A view is a virtual table based on the result of a SQL query.

### CREATE VIEW

```sql
CREATE VIEW high_salary_employees AS
SELECT first_name, salary
FROM employees
WHERE salary > 10000;
```

### Query a View

```sql
SELECT *
FROM high_salary_employees;
```

### CREATE OR REPLACE VIEW

Creates a view or replaces an existing view definition.

```sql
CREATE OR REPLACE VIEW employee_details AS
SELECT first_name, department_id
FROM employees;
```

### DROP VIEW

Deletes a view without deleting the original table data.

```sql
DROP VIEW employee_details;
```

### Why Use Views?

- Simplifies complex queries.
- Provides a reusable query structure.
- Can restrict access to selected columns or rows.
- Helps organize database logic.

---

# 24. Stored Procedures

A stored procedure is a collection of SQL statements stored in the database and executed when called.

### CREATE PROCEDURE

```sql
DELIMITER //

CREATE PROCEDURE GetEmployee(IN emp_id INT)
BEGIN
    SELECT *
    FROM employees
    WHERE employee_id = emp_id;
END //

DELIMITER ;
```

### Execute Procedure

```sql
CALL GetEmployee(101);
```

### Procedure with OUT Parameter

```sql
DELIMITER //

CREATE PROCEDURE GetEmployeeCount(OUT total INT)
BEGIN
    SELECT COUNT(*)
    INTO total
    FROM employees;
END //

DELIMITER ;
```

Execute:

```sql
CALL GetEmployeeCount(@total);

SELECT @total;
```

### Procedure Parameters

| Parameter | Meaning |
|---|---|
| IN | Passes a value into the procedure |
| OUT | Returns a value from the procedure |
| INOUT | Accepts a value and can modify and return it |

### DROP PROCEDURE

```sql
DROP PROCEDURE GetEmployee;
```

---

# 25. Functions and Triggers

## User-Defined Functions

A function is a reusable database routine that returns a value.

```sql
DELIMITER //

CREATE FUNCTION AnnualSalary(monthly_salary DECIMAL(10,2))
RETURNS DECIMAL(12,2)
DETERMINISTIC
BEGIN
    RETURN monthly_salary * 12;
END //

DELIMITER ;
```

Execute:

```sql
SELECT AnnualSalary(5000);
```

### Stored Procedure vs Function

| Procedure | Function |
|---|---|
| Called using CALL | Used inside SQL expressions |
| Can return multiple output parameters | Returns a single value |
| Can perform database operations | Has restrictions on database modifications |
| Used for executing a sequence of operations | Used for reusable calculations |

## Triggers

A trigger automatically executes when a specified database event occurs.

### CREATE TRIGGER

```sql
DELIMITER //

CREATE TRIGGER before_employee_insert
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    SET NEW.salary = IFNULL(NEW.salary, 0);
END //

DELIMITER ;
```

### Trigger Events

| Event | Meaning |
|---|---|
| BEFORE INSERT | Executes before inserting a row |
| AFTER INSERT | Executes after inserting a row |
| BEFORE UPDATE | Executes before updating a row |
| AFTER UPDATE | Executes after updating a row |
| BEFORE DELETE | Executes before deleting a row |
| AFTER DELETE | Executes after deleting a row |

### DROP TRIGGER

```sql
DROP TRIGGER before_employee_insert;
```

---

# 26. Transactions

A transaction is a group of SQL operations treated as a single logical unit.

## START TRANSACTION

Begins a transaction.

```sql
START TRANSACTION;
```

## COMMIT

Permanently saves the changes made in the transaction.

```sql
START TRANSACTION;

UPDATE employees
SET salary = 10000
WHERE employee_id = 101;

COMMIT;
```

## ROLLBACK

Reverts uncommitted changes.

```sql
START TRANSACTION;

UPDATE employees
SET salary = 10000
WHERE employee_id = 101;

ROLLBACK;
```

## SAVEPOINT

Creates a point within a transaction to which you can roll back.

```sql
START TRANSACTION;

UPDATE employees
SET salary = 10000
WHERE employee_id = 101;

SAVEPOINT sp1;

UPDATE employees
SET salary = 12000
WHERE employee_id = 102;

ROLLBACK TO SAVEPOINT sp1;

COMMIT;
```

### ACID Properties

| Property | Meaning |
|---|---|
| Atomicity | All operations succeed or the transaction is rolled back |
| Consistency | Data remains valid according to database rules |
| Isolation | Concurrent transactions are controlled to prevent interference |
| Durability | Committed changes persist |

### Transaction Isolation Levels

| Isolation Level | Description |
|---|---|
| READ UNCOMMITTED | Allows dirty reads |
| READ COMMITTED | Prevents dirty reads |
| REPEATABLE READ | Prevents dirty and non-repeatable reads |
| SERIALIZABLE | Provides the strictest standard isolation |

### Common Transaction Problems

- Dirty Read: Reading uncommitted changes made by another transaction.
- Non-repeatable Read: Reading the same row twice and getting different values.
- Phantom Read: Repeating a query and getting a different set of matching rows.

---

# 27. Performance Optimization

## EXPLAIN

Displays the execution plan of a SQL query.

```sql
EXPLAIN
SELECT *
FROM employees
WHERE salary > 10000;
```

### EXPLAIN ANALYZE

Executes the query and provides actual execution information along with the plan.

```sql
EXPLAIN ANALYZE
SELECT *
FROM employees
WHERE salary > 10000;
```

**Note:** Available in MySQL 8.0.18 and later.

### Optimization Techniques

1. Avoid SELECT * when only specific columns are required.
2. Create indexes on frequently filtered and joined columns.
3. Avoid unnecessary subqueries when a JOIN or CTE is clearer.
4. Use WHERE to filter rows as early as appropriate.
5. Avoid unnecessary DISTINCT operations.
6. Avoid functions on indexed columns in WHERE conditions when they prevent efficient index usage.
7. Use LIMIT when only a small result set is required.
8. Analyze query execution plans using EXPLAIN.
9. Avoid unnecessary indexes.
10. Use appropriate data types and constraints.

### Example: Avoid SELECT *

```sql
-- Less efficient when all columns are unnecessary
SELECT *
FROM employees;

-- Better when only two columns are required
SELECT first_name, salary
FROM employees;
```

---

# 28. SQL Execution Order

Understanding SQL execution order is important for writing and debugging complex queries.

Although SQL is written in a different order, its logical processing order is generally:

| Order | Clause | Purpose |
|---|---|---|
| 1 | FROM | Selects the source table |
| 2 | JOIN / ON | Combines tables |
| 3 | WHERE | Filters individual rows |
| 4 | GROUP BY | Groups records |
| 5 | HAVING | Filters groups |
| 6 | SELECT | Selects and calculates output columns |
| 7 | DISTINCT | Removes duplicate result rows |
| 8 | ORDER BY | Sorts the result |
| 9 | LIMIT / OFFSET | Restricts the result |

### Example

```sql
SELECT department_id, AVG(salary) AS avg_salary
FROM employees
WHERE salary > 3000
GROUP BY department_id
HAVING AVG(salary) > 8000
ORDER BY avg_salary DESC
LIMIT 5;
```

**Logical Execution:**

1. FROM employees
2. WHERE salary > 3000
3. GROUP BY department_id
4. HAVING AVG(salary) > 8000
5. SELECT department_id and AVG(salary)
6. ORDER BY avg_salary DESC
7. LIMIT 5

**Interview Tip:** A SELECT alias generally cannot be referenced in WHERE because WHERE is processed before SELECT. It can commonly be referenced in ORDER BY.

---

# 29. SQL Interview Quick Reference

Frequently asked SQL problems and their solutions.

## Q1. Find the Highest Salary

```sql
SELECT MAX(salary)
FROM employees;
```

## Q2. Find the Second Highest Distinct Salary

```sql
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
```

## Q3. Find the Nth Highest Salary

```sql
WITH ranked AS (
    SELECT
        salary,
        DENSE_RANK() OVER(
            ORDER BY salary DESC
        ) AS rnk
    FROM employees
)

SELECT DISTINCT salary
FROM ranked
WHERE rnk = 3;
```

Replace 3 with the required rank.

## Q4. Find Employees Earning More Than the Company Average

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

## Q5. Find Employees Earning More Than Their Department Average

```sql
SELECT e.*
FROM employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

## Q6. Find the Highest Paid Employee in Each Department

```sql
SELECT *
FROM (
    SELECT
        e.*,
        DENSE_RANK() OVER(
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rnk
    FROM employees e
) ranked
WHERE rnk = 1;
```

## Q7. Find Employees Earning More Than Their Managers

```sql
SELECT
    e.first_name AS employee_name,
    e.salary AS employee_salary,
    m.first_name AS manager_name,
    m.salary AS manager_salary
FROM employees e
JOIN employees m
ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;
```

## Q8. Find Duplicate Records Based on Email

```sql
SELECT email, COUNT(*) AS total
FROM employees
GROUP BY email
HAVING COUNT(*) > 1;
```

## Q9. Find Departments Having More Than 5 Employees

```sql
SELECT department_id, COUNT(*) AS total_employees
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 5;
```

## Q10. Find Employees Who Have No Manager

```sql
SELECT *
FROM employees
WHERE manager_id IS NULL;
```

## Q11. Find Employees Who Do Not Belong to Any Department

```sql
SELECT e.*
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

## Q12. Find the Top 3 Salaries in Each Department

```sql
WITH ranked AS (
    SELECT
        first_name,
        department_id,
        salary,
        DENSE_RANK() OVER(
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rnk
    FROM employees
)

SELECT *
FROM ranked
WHERE rnk <= 3;
```

## Q13. Count Employees in Each Department

```sql
SELECT department_id, COUNT(*) AS total_employees
FROM employees
GROUP BY department_id;
```

## Q14. Find Employees Hired in the Current Year

```sql
SELECT *
FROM employees
WHERE YEAR(hire_date) = YEAR(CURDATE());
```

## Q15. Find the Difference Between Maximum and Minimum Salary

```sql
SELECT MAX(salary) - MIN(salary) AS salary_difference
FROM employees;
```

---

# 30. SQL Learning Roadmap

A structured roadmap for learning SQL from beginner to advanced level.

### Beginner

- [ ] SELECT
- [ ] SELECT DISTINCT
- [ ] WHERE
- [ ] AND / OR / NOT
- [ ] IN / NOT IN
- [ ] BETWEEN
- [ ] LIKE / NOT LIKE
- [ ] IS NULL / IS NOT NULL
- [ ] ORDER BY
- [ ] LIMIT / OFFSET
- [ ] Column Aliases
- [ ] Aggregate Functions

### Intermediate

- [ ] GROUP BY
- [ ] HAVING
- [ ] CASE WHEN
- [ ] INNER JOIN
- [ ] LEFT JOIN
- [ ] RIGHT JOIN
- [ ] FULL OUTER JOIN
- [ ] CROSS JOIN
- [ ] SELF JOIN
- [ ] Multiple JOINs
- [ ] UNION / UNION ALL
- [ ] Single Row Subqueries
- [ ] Multiple Row Subqueries
- [ ] Correlated Subqueries
- [ ] EXISTS / NOT EXISTS
- [ ] ANY / ALL

### Advanced

- [ ] Common Table Expressions (CTEs)
- [ ] Recursive CTEs
- [ ] Window Functions
- [ ] ROW_NUMBER()
- [ ] RANK()
- [ ] DENSE_RANK()
- [ ] NTILE()
- [ ] LAG() / LEAD()
- [ ] FIRST_VALUE() / LAST_VALUE()
- [ ] Running Totals
- [ ] Conditional Aggregation
- [ ] Advanced Date and String Functions

### Database Development

- [ ] CREATE / ALTER / DROP
- [ ] Primary and Foreign Keys
- [ ] Constraints
- [ ] INSERT / UPDATE / DELETE
- [ ] Indexes
- [ ] Views
- [ ] Stored Procedures
- [ ] User-Defined Functions
- [ ] Triggers
- [ ] Transactions
- [ ] ACID Properties
- [ ] Isolation Levels

### Performance and Optimization

- [ ] EXPLAIN
- [ ] EXPLAIN ANALYZE
- [ ] Index Optimization
- [ ] Query Optimization
- [ ] Understanding Execution Plans

---

# ⭐ Additional SQL Learning Resources

- [MySQL Official Documentation](https://dev.mysql.com/doc/)
- [W3Schools SQL Tutorial](https://www.w3schools.com/sql/)
- [SQLBolt - Interactive SQL Lessons](https://sqlbolt.com/)
- [LeetCode SQL 50](https://leetcode.com/studyplan/top-sql-50/)
- [HackerRank SQL Practice](https://www.hackerrank.com/domains/sql)

---

## 💡 Final Note

SQL is best learned through consistent practice. Focus on understanding how queries work rather than memorizing their syntax.

Practice JOINs, GROUP BY, subqueries, CTEs, and window functions regularly because they are especially important for SQL interviews.

**Happy Querying! 🚀**
