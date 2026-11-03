# 📘 Complete Oracle SQL Cheat Sheet

A practical reference for learning SQL using **Oracle Database and Oracle SQL Developer**. Covers beginner to advanced SQL, PL/SQL, database objects, queries, explanations, and interview problems.

**Database:** Oracle Database  
**IDE:** Oracle SQL Developer  
**Level:** Beginner to Advanced

> **Important:** SQL Developer is the tool used to connect to Oracle Database. SQL syntax is determined by the Oracle Database version, not by the editor.

---

# 📑 Table of Contents

1. [SQL Basics](#1-sql-basics)
2. [Oracle Database and Schema Management](#2-oracle-database-and-schema-management)
3. [Data Types](#3-data-types)
4. [Constraints](#4-constraints)
5. [SELECT Statements](#5-select-statements)
6. [Filtering and Operators](#6-filtering-and-operators)
7. [Sorting and Fetching Rows](#7-sorting-and-fetching-rows)
8. [Aggregate Functions](#8-aggregate-functions)
9. [GROUP BY and HAVING](#9-group-by-and-having)
10. [CASE Expressions](#10-case-expressions)
11. [Data Manipulation](#11-data-manipulation)
12. [Joins](#12-joins)
13. [Set Operators](#13-set-operators)
14. [Subqueries](#14-subqueries)
15. [EXISTS and NOT EXISTS](#15-exists-and-not-exists)
16. [Common Table Expressions](#16-common-table-expressions)
17. [Window and Analytic Functions](#17-window-and-analytic-functions)
18. [Ranking Functions](#18-ranking-functions)
19. [String Functions](#19-string-functions)
20. [Numeric Functions](#20-numeric-functions)
21. [Date and Time Functions](#21-date-and-time-functions)
22. [NULL Handling](#22-null-handling)
23. [Indexes](#23-indexes)
24. [Views](#24-views)
25. [Sequences and Identity Columns](#25-sequences-and-identity-columns)
26. [PL/SQL Basics](#26-plsql-basics)
27. [Stored Procedures](#27-stored-procedures)
28. [Functions](#28-functions)
29. [Cursors](#29-cursors)
30. [Triggers](#30-triggers)
31. [Transactions](#31-transactions)
32. [Performance and Execution Plans](#32-performance-and-execution-plans)
33. [SQL Logical Processing Order](#33-sql-logical-processing-order)
34. [Oracle SQL Interview Questions](#34-oracle-sql-interview-questions)
35. [SQL Learning Roadmap](#35-sql-learning-roadmap)

---

# 1. SQL Basics

SQL stands for **Structured Query Language**. It is used to retrieve, insert, update, delete, and manage data in relational databases.

### SQL Command Categories

| Category | Full Form | Purpose | Commands |
|---|---|---|---|
| DDL | Data Definition Language | Defines database objects | CREATE, ALTER, DROP, TRUNCATE, RENAME |
| DML | Data Manipulation Language | Changes table data | INSERT, UPDATE, DELETE, MERGE |
| DQL | Data Query Language | Retrieves data | SELECT |
| DCL | Data Control Language | Manages privileges | GRANT, REVOKE |
| TCL | Transaction Control Language | Manages transactions | COMMIT, ROLLBACK, SAVEPOINT |

---

# 2. Oracle Database and Schema Management

In Oracle, a user generally owns a **schema**. A schema contains objects such as tables, views, indexes, sequences, procedures, and functions.

Unlike MySQL, you normally do not create and switch between databases using `CREATE DATABASE` and `USE`.

### Show the Current User

```sql
SELECT USER
FROM dual;
```

### Show the Current Schema

```sql
SELECT SYS_CONTEXT('USERENV', 'CURRENT_SCHEMA')
FROM dual;
```

### Show Tables Owned by the Current User

```sql
SELECT table_name
FROM user_tables
ORDER BY table_name;
```

### Show Tables Accessible to the Current User

```sql
SELECT owner, table_name
FROM all_tables
ORDER BY owner, table_name;
```

### Describe a Table

```sql
DESC employees;
```

Or query its metadata:

```sql
SELECT column_name, data_type, nullable
FROM user_tab_columns
WHERE table_name = 'EMPLOYEES'
ORDER BY column_id;
```

**Note:** Oracle stores unquoted object names in uppercase in the data dictionary.

### Create a User (requires appropriate privileges)

```sql
CREATE USER practice_user IDENTIFIED BY password123;

GRANT CREATE SESSION, CREATE TABLE, CREATE VIEW,
      CREATE SEQUENCE, CREATE PROCEDURE, CREATE TRIGGER
TO practice_user;
```

A DBA may also need to assign an appropriate tablespace quota before the user can create objects.

### Change the Current Schema

```sql
ALTER SESSION SET CURRENT_SCHEMA = practice_user;
```

This changes name resolution; it does not log you in as that user or grant additional privileges.

### Drop a User (administrative operation)

```sql
DROP USER practice_user CASCADE;
```

`CASCADE` also removes objects owned by that user. Use carefully.

---

# 3. Data Types

Oracle uses different data types from MySQL.

| Oracle Data Type | Meaning | Example |
|---|---|---|
| NUMBER(p,s) | Numeric value with precision and scale | NUMBER(10,2) |
| INTEGER | Whole number (subtype of NUMBER) | 100 |
| BINARY_FLOAT | Single-precision floating-point | 12.5 |
| BINARY_DOUBLE | Double-precision floating-point | 12.5 |
| CHAR(n) | Fixed-length character string | CHAR(5) |
| VARCHAR2(n) | Variable-length character string | VARCHAR2(50) |
| NVARCHAR2(n) | Variable-length Unicode string | NVARCHAR2(50) |
| CLOB | Large character data | Long description |
| BLOB | Binary large object | Image or file data |
| DATE | Date and time to seconds | DATE |
| TIMESTAMP | Date and time with fractional seconds | TIMESTAMP |
| TIMESTAMP WITH TIME ZONE | Timestamp with time zone | TIMESTAMP WITH TIME ZONE |
| RAW(n) | Raw binary data | RAW(16) |

**Important Oracle differences:**
- Use `VARCHAR2` for variable-length character data.
- Oracle `DATE` stores both date and time to seconds.
- Oracle does not have a separate SQL `TIME` data type.
- `BOOLEAN` is available in PL/SQL. SQL table-column support depends on Oracle Database version; Oracle Database 23ai supports SQL BOOLEAN columns.

### CREATE TABLE

```sql
CREATE TABLE employees (
    employee_id NUMBER PRIMARY KEY,
    first_name VARCHAR2(50),
    salary NUMBER(10,2),
    department_id NUMBER
);
```

---

# 4. Constraints

Constraints enforce rules on table data.

### PRIMARY KEY

Uniquely identifies every row. A primary key cannot contain NULL or duplicate values.

```sql
CREATE TABLE students (
    student_id NUMBER PRIMARY KEY,
    name VARCHAR2(50)
);
```

### FOREIGN KEY

Creates a relationship with a primary key or unique key in another table.

```sql
CREATE TABLE orders (
    order_id NUMBER PRIMARY KEY,
    student_id NUMBER,
    CONSTRAINT fk_orders_student
        FOREIGN KEY (student_id)
        REFERENCES students(student_id)
);
```

### UNIQUE

Prevents duplicate values in a column or combination of columns.

```sql
CREATE TABLE users (
    user_id NUMBER PRIMARY KEY,
    email VARCHAR2(100) UNIQUE
);
```

Oracle UNIQUE constraints allow NULL values; the exact behavior depends on whether one or multiple columns are involved.

### NOT NULL

Prevents a column from storing NULL.

```sql
CREATE TABLE employees (
    employee_id NUMBER PRIMARY KEY,
    first_name VARCHAR2(50) NOT NULL
);
```

### CHECK

Ensures that values satisfy a condition.

```sql
CREATE TABLE employees (
    employee_id NUMBER PRIMARY KEY,
    salary NUMBER(10,2) CHECK (salary > 0)
);
```

### DEFAULT

Provides a value when an INSERT omits the column.

```sql
CREATE TABLE employees (
    employee_id NUMBER PRIMARY KEY,
    country VARCHAR2(50) DEFAULT 'India'
);
```

### Add a Constraint Later

```sql
ALTER TABLE employees
ADD CONSTRAINT chk_salary CHECK (salary >= 0);
```

### Drop a Constraint

```sql
ALTER TABLE employees
DROP CONSTRAINT chk_salary;
```

---

# 5. SELECT Statements

`SELECT` retrieves data from one or more tables.

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

**Meaning:** Retrieves all columns.

### Select a Literal Value

Oracle uses the special one-row table `DUAL` for expressions that do not need a real table.

```sql
SELECT 'Hello Oracle' AS message
FROM dual;
```

```sql
SELECT 10 + 20 AS result
FROM dual;
```

### DISTINCT

Removes duplicate result rows.

```sql
SELECT DISTINCT department_id
FROM employees;
```

### DISTINCT on Multiple Columns

```sql
SELECT DISTINCT department_id, job_id
FROM employees;
```

**Meaning:** Returns unique combinations of department_id and job_id.

### Column Aliases

```sql
SELECT
    first_name AS employee_name,
    salary AS monthly_salary
FROM employees;
```

### Calculated Columns

```sql
SELECT
    first_name,
    salary,
    salary * 12 AS annual_salary
FROM employees;
```

### Concatenation Operator

Oracle uses `||` to concatenate strings.

```sql
SELECT first_name || ' ' || last_name AS full_name
FROM employees;
```

`CONCAT()` is also available, but it accepts two arguments at a time.

---

# 6. Filtering and Operators

`WHERE` filters rows before grouping.

### Comparison Operators

| Operator | Meaning |
|---|---|
| = | Equal to |
| != or <> | Not equal to |
| > | Greater than |
| < | Less than |
| >= | Greater than or equal to |
| <= | Less than or equal to |

### WHERE

```sql
SELECT *
FROM employees
WHERE salary > 5000;
```

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

Checks whether a value matches any value in a list.

```sql
SELECT *
FROM employees
WHERE department_id IN (10, 20, 30);
```

### NOT IN

Excludes specified values.

```sql
SELECT *
FROM employees
WHERE department_id NOT IN (10, 20);
```

**Caution:** If a `NOT IN` subquery returns NULL, the comparison can produce UNKNOWN and return no rows. `NOT EXISTS` is often safer for anti-join logic.

### BETWEEN

Checks an inclusive range.

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 5000 AND 10000;
```

Both 5000 and 10000 are included.

### LIKE

Searches for a text pattern.

```sql
SELECT *
FROM employees
WHERE first_name LIKE 'A%';
```

| Pattern | Meaning |
|---|---|
| `'A%'` | Starts with A |
| `'%A'` | Ends with A |
| `'%A%'` | Contains A |
| `'A_'` | A followed by exactly one character |
| `'_A%'` | Second character is A |
| `'A%B'` | Starts with A and ends with B |

- `%` matches zero or more characters.
- `_` matches exactly one character.

### NOT LIKE

```sql
SELECT *
FROM employees
WHERE first_name NOT LIKE 'A%';
```

### IS NULL

```sql
SELECT *
FROM employees
WHERE manager_id IS NULL;
```

### IS NOT NULL

```sql
SELECT *
FROM employees
WHERE manager_id IS NOT NULL;
```

### EXISTS in a Condition

```sql
SELECT *
FROM employees e
WHERE EXISTS (
    SELECT 1
    FROM departments d
    WHERE d.department_id = e.department_id
);
```

---

# 7. Sorting and Fetching Rows

### ORDER BY ASC

Sorts from lowest to highest or alphabetically ascending.

```sql
SELECT *
FROM employees
ORDER BY salary ASC;
```

### ORDER BY DESC

Sorts from highest to lowest.

```sql
SELECT *
FROM employees
ORDER BY salary DESC;
```

### Multiple Column Sorting

```sql
SELECT *
FROM employees
ORDER BY department_id ASC, salary DESC;
```

### FETCH FIRST

Returns a limited number of rows. Supported in Oracle 12c and later.

```sql
SELECT *
FROM employees
ORDER BY salary DESC
FETCH FIRST 5 ROWS ONLY;
```

### OFFSET and FETCH

Skips rows and returns the next specified number.

```sql
SELECT *
FROM employees
ORDER BY employee_id
OFFSET 10 ROWS
FETCH NEXT 5 ROWS ONLY;
```

### WITH TIES

Includes additional rows tied with the last fetched row according to the ORDER BY keys.

```sql
SELECT *
FROM employees
ORDER BY salary DESC
FETCH FIRST 5 ROWS WITH TIES;
```

### Find the Highest Salary

```sql
SELECT MAX(salary)
FROM employees;
```

Or retrieve the employee row:

```sql
SELECT *
FROM employees
ORDER BY salary DESC
FETCH FIRST 1 ROW ONLY;
```

### Find the Second Highest Distinct Salary

```sql
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
OFFSET 1 ROW
FETCH NEXT 1 ROW ONLY;
```

**Older Oracle versions:** Use `ROWNUM` with an ordered inline view when the row-limiting clause is unavailable.

```sql
SELECT *
FROM (
    SELECT *
    FROM employees
    ORDER BY salary DESC
)
WHERE ROWNUM <= 5;
```

---

# 8. Aggregate Functions

Aggregate functions calculate a result from multiple rows.

### COUNT(*)

Counts all rows.

```sql
SELECT COUNT(*)
FROM employees;
```

### COUNT(column)

Counts non-NULL values.

```sql
SELECT COUNT(commission_pct)
FROM employees;
```

### COUNT(DISTINCT column)

Counts unique non-NULL values.

```sql
SELECT COUNT(DISTINCT department_id)
FROM employees;
```

### SUM()

```sql
SELECT SUM(salary)
FROM employees;
```

Returns the total salary.

### AVG()

```sql
SELECT AVG(salary)
FROM employees;
```

Calculates the average of non-NULL values.

### MIN()

```sql
SELECT MIN(salary)
FROM employees;
```

Returns the smallest value.

### MAX()

```sql
SELECT MAX(salary)
FROM employees;
```

Returns the largest value.

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

# 9. GROUP BY and HAVING

### GROUP BY

Groups rows that have the same values in the specified columns.

```sql
SELECT department_id, COUNT(*) AS total_employees
FROM employees
GROUP BY department_id;
```

**Meaning:** Creates one group per department and counts employees in each group.

### GROUP BY Multiple Columns

```sql
SELECT department_id, job_id, COUNT(*) AS total
FROM employees
GROUP BY department_id, job_id;
```

Groups by each unique combination of department_id and job_id.

### HAVING

Filters groups after grouping.

```sql
SELECT department_id, AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 8000;
```

**Meaning:** Displays departments whose average salary is greater than 8000.

### WHERE vs HAVING

| WHERE | HAVING |
|---|---|
| Filters individual rows | Filters groups |
| Applied before GROUP BY | Applied after GROUP BY |
| Filters source records | Filters aggregate results |
| Cannot use aggregate functions directly in its condition | Commonly uses aggregate functions |

### WHERE and HAVING Together

```sql
SELECT department_id, AVG(salary) AS avg_salary
FROM employees
WHERE salary > 3000
GROUP BY department_id
HAVING AVG(salary) > 8000;
```

1. WHERE removes employees earning 3000 or less.
2. GROUP BY groups the remaining employees by department.
3. AVG calculates each department's average.
4. HAVING retains departments with an average above 8000.

---

# 10. CASE Expressions

`CASE` provides conditional logic similar to if-else statements.

### Searched CASE

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

### Simple CASE

Compares one expression against multiple values.

```sql
SELECT
    department_id,
    CASE department_id
        WHEN 10 THEN 'HR'
        WHEN 20 THEN 'IT'
        WHEN 30 THEN 'Finance'
        ELSE 'Other'
    END AS department_name
FROM employees;
```

### CASE with GROUP BY

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

Counts employees earning more than 5000 in each department.

### Conditional Aggregation

```sql
SELECT
    COUNT(CASE WHEN salary > 5000 THEN 1 END) AS high_salary,
    COUNT(CASE WHEN salary <= 5000 THEN 1 END) AS low_salary
FROM employees;
```

---

# 11. Data Manipulation

## INSERT

Adds records to a table.

### Insert One Record

```sql
INSERT INTO employees (employee_id, first_name, salary)
VALUES (101, 'Piyush', 8000);
```

### Insert Multiple Records

Oracle supports multi-row insertion using `INSERT ALL`.

```sql
INSERT ALL
    INTO employees (employee_id, first_name, salary)
    VALUES (102, 'Amit', 7000)
    INTO employees (employee_id, first_name, salary)
    VALUES (103, 'Rahul', 9000)
SELECT 1 FROM dual;
```

### INSERT INTO SELECT

Copies rows from one table into another.

```sql
INSERT INTO employee_backup (employee_id, first_name, salary)
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

**Important:** Without WHERE, every row is updated.

## DELETE

Removes selected rows.

```sql
DELETE FROM employees
WHERE employee_id = 101;
```

### Delete All Rows

```sql
DELETE FROM employees;
```

The table structure remains.

## MERGE

Performs an INSERT or UPDATE depending on whether a matching row exists. This is often called an upsert.

```sql
MERGE INTO employee_target t
USING employee_source s
ON (t.employee_id = s.employee_id)
WHEN MATCHED THEN
    UPDATE SET t.salary = s.salary
WHEN NOT MATCHED THEN
    INSERT (employee_id, salary)
    VALUES (s.employee_id, s.salary);
```

## DELETE vs TRUNCATE vs DROP

| Command | Meaning |
|---|---|
| DELETE | Removes selected or all rows; supports WHERE |
| TRUNCATE | Removes all rows; does not support WHERE |
| DROP | Removes the table object itself |

**Oracle note:** DDL statements such as TRUNCATE, DROP, and CREATE cause implicit commits. Do not expect to roll them back like ordinary uncommitted DML.

---

# 12. Joins

Joins combine rows from two or more tables using related columns.

Assume these tables:

### employees

| employee_id | name | department_id |
|---|---|---|
| 1 | Amit | 10 |
| 2 | Rahul | 20 |
| 3 | Piyush | 30 |
| 4 | Neha | NULL |

### departments

| department_id | department_name |
|---|---|
| 10 | HR |
| 20 | IT |
| 40 | Finance |

## 12.1 INNER JOIN

Returns only rows with matching values in both tables.

```sql
SELECT *
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id;
```

**Meaning:**
- Takes matching rows from the first table.
- Takes matching rows from the second table.
- Unmatched rows from either table are excluded.

**Result:** Amit and Rahul appear. Piyush and Neha do not have matching departments.

## 12.2 LEFT JOIN / LEFT OUTER JOIN

Returns every row from the left table and matching rows from the right table.

```sql
SELECT *
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id;
```

**Meaning:**
- Takes ALL rows from the first table (`employees`).
- Takes only matching rows from the second table (`departments`).
- If there is no match, columns from the right table contain NULL.

**Result:** All four employees appear. Piyush and Neha have NULL department details.

## 12.3 RIGHT JOIN / RIGHT OUTER JOIN

Returns every row from the right table and matching rows from the left table.

```sql
SELECT *
FROM employees e
RIGHT JOIN departments d
ON e.department_id = d.department_id;
```

**Meaning:**
- Takes ALL rows from the second table (`departments`).
- Takes only matching rows from the first table (`employees`).
- If there is no match, columns from the left table contain NULL.

**Result:** HR, IT, and Finance appear. Finance has NULL employee details.

## 12.4 FULL OUTER JOIN

Returns all rows from both tables. Matching rows are combined; unmatched columns are NULL.

```sql
SELECT *
FROM employees e
FULL OUTER JOIN departments d
ON e.department_id = d.department_id;
```

Unlike MySQL, Oracle supports FULL OUTER JOIN directly.

## 12.5 CROSS JOIN

Returns every possible combination of rows from both tables.

```sql
SELECT *
FROM employees
CROSS JOIN departments;
```

If the first table has 4 rows and the second has 3 rows, the result has 12 rows.

## 12.6 SELF JOIN

Joins a table with itself. Useful for employee-manager relationships.

```sql
SELECT
    e.first_name AS employee_name,
    m.first_name AS manager_name
FROM employees e
JOIN employees m
ON e.manager_id = m.employee_id;
```

- `e` represents the employee.
- `m` represents the manager.
- Both aliases refer to the same table.

## 12.7 Multiple JOINs

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

Combines information from employees, departments, and locations.

## 12.8 JOIN with WHERE

```sql
SELECT e.first_name, d.department_name
FROM employees e
JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_name = 'IT';
```

## 12.9 LEFT JOIN with IS NULL

Finds rows in the left table with no matching row in the right table.

```sql
SELECT e.*
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

This includes employees with a NULL department_id as well as employees whose department_id has no match.

### Find Departments Without Employees

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

# 13. Set Operators

Set operators combine the results of SELECT statements. The queries must return the same number of columns, in compatible data types and corresponding positions.

## UNION

Combines results and removes duplicate rows.

```sql
SELECT department_id FROM employees
UNION
SELECT department_id FROM departments;
```

## UNION ALL

Combines results and retains duplicates.

```sql
SELECT department_id FROM employees
UNION ALL
SELECT department_id FROM departments;
```

## INTERSECT

Returns rows common to both query results.

```sql
SELECT department_id FROM employees
INTERSECT
SELECT department_id FROM departments;
```

## MINUS

Returns rows from the first query that do not appear in the second query.

```sql
SELECT department_id FROM departments
MINUS
SELECT department_id FROM employees;
```

### Set Operator Comparison

| Operator | Meaning |
|---|---|
| UNION | Combines results and removes duplicates |
| UNION ALL | Combines results and keeps duplicates |
| INTERSECT | Returns common rows |
| MINUS | Returns rows in the first query but not the second |

**Note:** Oracle uses `MINUS` for this operation in widely used versions. Check the syntax supported by your exact Oracle Database release before using newer alternatives.

---

# 14. Subqueries

A subquery is a query nested inside another SQL statement.

## 14.1 Single-Row Subquery

Returns one row. Common comparison operators include `=`, `>`, `<`, `>=`, and `<=`.

```sql
SELECT first_name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

**Meaning:** Finds employees earning more than the company average.

## 14.2 Multiple-Row Subquery

Returns multiple rows. Use operators such as IN, ANY, and ALL.

```sql
SELECT first_name
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM departments
    WHERE location_id = 1700
);
```

## 14.3 Scalar Subquery

Returns one value and can be used in a SELECT list or expression.

```sql
SELECT
    first_name,
    salary,
    (SELECT AVG(salary) FROM employees) AS company_avg_salary
FROM employees;
```

## 14.4 Multiple-Column Subquery

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

## 14.5 Correlated Subquery

References a column from the outer query.

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

## 14.6 Subquery in FROM

Uses a subquery as an inline view.

```sql
SELECT *
FROM (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
) dept_summary
WHERE avg_salary > 8000;
```

## 14.7 ANY

The comparison must be true for at least one value returned by the subquery.

```sql
SELECT *
FROM employees
WHERE salary > ANY (
    SELECT salary
    FROM employees
    WHERE department_id = 20
);
```

## 14.8 ALL

The comparison must be true for every value returned by the subquery.

```sql
SELECT *
FROM employees
WHERE salary > ALL (
    SELECT salary
    FROM employees
    WHERE department_id = 20
);
```

### ANY vs ALL

| ANY | ALL |
|---|---|
| At least one comparison must be true | Every comparison must be true |
| Similar to OR logic | Similar to AND logic |

---

# 15. EXISTS and NOT EXISTS

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
| Tests whether matching rows exist | Tests membership in a list or subquery result |
| Often used with correlated subqueries | Often used with a list or single-column subquery |
| Useful for existence checks | Useful for value comparisons |
| Usually unaffected by NULL values in the selected subquery column when checking a matching predicate | `NOT IN` can behave unexpectedly if the subquery returns NULL |

---

# 16. Common Table Expressions (CTEs)

A CTE creates a named result set available to the statement that follows it.

### Basic CTE

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

### Multiple CTEs

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

### Recursive Subquery Factoring

Oracle supports recursive subquery factoring. Unlike MySQL and some other systems, Oracle does **not** put the keyword `RECURSIVE` after `WITH`.

```sql
WITH numbers (n) AS (
    SELECT 1 FROM dual
    UNION ALL
    SELECT n + 1
    FROM numbers
    WHERE n < 10
)
SELECT n
FROM numbers;
```

This generates numbers from 1 to 10.

For hierarchical data, Oracle also provides `START WITH ... CONNECT BY`.

```sql
SELECT employee_id, manager_id, LEVEL
FROM employees
START WITH manager_id IS NULL
CONNECT BY PRIOR employee_id = manager_id;
```

---

# 17. Window and Analytic Functions

Oracle commonly calls window functions **analytic functions**. They calculate across related rows without collapsing the result into one row per group.

## OVER()

Defines the analytic window.

```sql
SELECT
    first_name,
    salary,
    AVG(salary) OVER() AS company_avg
FROM employees;
```

Displays every employee alongside the company-wide average salary.

## PARTITION BY

Divides rows into groups for the analytic calculation without removing individual rows.

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

Calculates the average salary separately for each department.

## ORDER BY inside OVER()

Defines the order used by the analytic function.

```sql
SELECT
    first_name,
    salary,
    SUM(salary) OVER(
        ORDER BY employee_id
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS running_total
FROM employees;
```

Calculates a running total.

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

Returns the first value in the analytic window.

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

**Note:** The window frame matters. Without an appropriate frame, LAST_VALUE may return the current row's value rather than the last value in the entire partition.

---

# 18. Ranking Functions

Ranking functions assign row numbers or ranks based on ordering.

## ROW_NUMBER()

Assigns a unique sequential number to every row.

```sql
SELECT
    first_name,
    salary,
    ROW_NUMBER() OVER(
        ORDER BY salary DESC
    ) AS row_num
FROM employees;
```

If salaries tie, rows still receive different row numbers. Add a second ORDER BY column for deterministic ordering.

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

Example: `1, 2, 2, 4`.

## DENSE_RANK()

Assigns the same rank to tied values without skipping subsequent ranks.

```sql
SELECT
    first_name,
    salary,
    DENSE_RANK() OVER(
        ORDER BY salary DESC
    ) AS salary_rank
FROM employees;
```

Example: `1, 2, 2, 3`.

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

Divides employees into four groups.

### Ranking Comparison

| Function | Ties | Skips Rank |
|---|---|---|
| ROW_NUMBER() | Gives unique numbers | No |
| RANK() | Same rank | Yes |
| DENSE_RANK() | Same rank | No |
| NTILE(n) | Divides rows into buckets | Not a traditional ranking function |

### Second Highest Distinct Salary

```sql
SELECT salary
FROM (
    SELECT
        salary,
        DENSE_RANK() OVER(ORDER BY salary DESC) AS rnk
    FROM employees
)
WHERE rnk = 2
GROUP BY salary;
```

### Top 3 Salaries in Each Department

```sql
SELECT *
FROM (
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
WHERE rnk <= 3;
```

---

# 19. String Functions

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

### INITCAP()

Converts the first letter of each word to uppercase.

```sql
SELECT INITCAP('hello oracle sql')
FROM dual;
```

### LENGTH()

Returns the number of characters.

```sql
SELECT LENGTH(first_name)
FROM employees;
```

### SUBSTR()

Extracts part of a string.

```sql
SELECT SUBSTR(first_name, 1, 3)
FROM employees;
```

Returns three characters starting at position 1. Oracle string positions are 1-based.

### INSTR()

Returns the position of a substring.

```sql
SELECT INSTR('Hello World', 'World')
FROM dual;
```

### TRIM()

Removes leading and trailing characters, spaces by default.

```sql
SELECT TRIM('  Piyush  ')
FROM dual;
```

### LTRIM()

Removes characters from the left.

```sql
SELECT LTRIM('   Piyush')
FROM dual;
```

### RTRIM()

Removes characters from the right.

```sql
SELECT RTRIM('Piyush   ')
FROM dual;
```

### REPLACE()

Replaces occurrences of a substring.

```sql
SELECT REPLACE('Hello World', 'World', 'Oracle')
FROM dual;
```

### CONCAT()

Concatenates two strings.

```sql
SELECT CONCAT(first_name, last_name)
FROM employees;
```

For multiple strings, use `||`:

```sql
SELECT first_name || ' ' || last_name
FROM employees;
```

### LPAD() and RPAD()

Pads a string to a specified length.

```sql
SELECT LPAD('123', 5, '0')
FROM dual;
```

```sql
SELECT RPAD('123', 5, '0')
FROM dual;
```

### REVERSE

Oracle does not provide the same built-in `REVERSE()` function available in some other databases. Use a custom function or another appropriate approach if reversal is required.

---

# 20. Numeric Functions

### ABS()

Returns the absolute value.

```sql
SELECT ABS(-100)
FROM dual;
```

### ROUND()

Rounds a number.

```sql
SELECT ROUND(123.456, 2)
FROM dual;
```

### TRUNC()

Truncates a number without rounding.

```sql
SELECT TRUNC(123.456, 2)
FROM dual;
```

### CEIL()

Returns the smallest integer greater than or equal to the number.

```sql
SELECT CEIL(12.3)
FROM dual;
```

### FLOOR()

Returns the largest integer less than or equal to the number.

```sql
SELECT FLOOR(12.9)
FROM dual;
```

### MOD()

Returns the remainder.

```sql
SELECT MOD(10, 3)
FROM dual;
```

### POWER()

Raises a number to a specified power.

```sql
SELECT POWER(2, 3)
FROM dual;
```

### SQRT()

Returns the square root.

```sql
SELECT SQRT(25)
FROM dual;
```

---

# 21. Date and Time Functions

Oracle `DATE` stores date and time to seconds. `TIMESTAMP` supports fractional seconds.

### SYSDATE

Returns the current date and time from the database server.

```sql
SELECT SYSDATE
FROM dual;
```

### SYSTIMESTAMP

Returns a timestamp with fractional seconds and time zone information.

```sql
SELECT SYSTIMESTAMP
FROM dual;
```

### CURRENT_DATE

Returns the current date and time in the session time zone.

```sql
SELECT CURRENT_DATE
FROM dual;
```

### EXTRACT()

Extracts a year, month, day, or other supported date/time component.

```sql
SELECT EXTRACT(YEAR FROM hire_date)
FROM employees;
```

```sql
SELECT EXTRACT(MONTH FROM hire_date)
FROM employees;
```

### TO_CHAR()

Converts a date or number to formatted text.

```sql
SELECT TO_CHAR(hire_date, 'DD-MM-YYYY')
FROM employees;
```

```sql
SELECT TO_CHAR(SYSDATE, 'DD-MON-YYYY HH24:MI:SS')
FROM dual;
```

### TO_DATE()

Converts character data to a DATE using a format mask.

```sql
SELECT TO_DATE('03-11-2026', 'DD-MM-YYYY')
FROM dual;
```

### ADD_MONTHS()

Adds or subtracts months.

```sql
SELECT ADD_MONTHS(SYSDATE, 3)
FROM dual;
```

### MONTHS_BETWEEN()

Returns the number of months between two dates.

```sql
SELECT MONTHS_BETWEEN(SYSDATE, hire_date)
FROM employees;
```

### TRUNC(date)

Removes the time component or truncates to a specified date unit.

```sql
SELECT TRUNC(SYSDATE)
FROM dual;
```

First day of the current month:

```sql
SELECT TRUNC(SYSDATE, 'MM')
FROM dual;
```

### LAST_DAY()

Returns the last day of the month.

```sql
SELECT LAST_DAY(SYSDATE)
FROM dual;
```

### NEXT_DAY()

Returns the next specified weekday.

```sql
SELECT NEXT_DAY(SYSDATE, 'MONDAY')
FROM dual;
```

### Date Arithmetic

Add 7 days:

```sql
SELECT SYSDATE + 7
FROM dual;
```

Subtract 10 days:

```sql
SELECT SYSDATE - 10
FROM dual;
```

Difference between two DATE values is expressed in days:

```sql
SELECT SYSDATE - hire_date AS days_employed
FROM employees;
```

### Find Employees Hired in the Current Year

```sql
SELECT *
FROM employees
WHERE hire_date >= TRUNC(SYSDATE, 'YYYY')
AND hire_date < ADD_MONTHS(TRUNC(SYSDATE, 'YYYY'), 12);
```

Using a date range avoids applying a function to every hire_date value in the filter.

---

# 22. NULL Handling

NULL represents missing or unknown data. It is not the same as zero or an empty string.

### NVL()

Returns an alternative value when the first expression is NULL.

```sql
SELECT NVL(commission_pct, 0)
FROM employees;
```

### NVL2()

Returns one value if the expression is not NULL and another if it is NULL.

```sql
SELECT NVL2(commission_pct, 'Has Commission', 'No Commission')
FROM employees;
```

### COALESCE()

Returns the first non-NULL expression.

```sql
SELECT COALESCE(commission_pct, salary, 0)
FROM employees;
```

### NULLIF()

Returns NULL if two expressions are equal; otherwise returns the first expression.

```sql
SELECT NULLIF(salary, 0)
FROM employees;
```

### CASE for NULL Handling

```sql
SELECT
    first_name,
    CASE
        WHEN commission_pct IS NULL THEN 0
        ELSE commission_pct
    END AS commission
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

Comparisons with NULL using ordinary equality operators produce UNKNOWN, not TRUE or FALSE.

**Oracle-specific note:** Oracle treats a zero-length character string (`''`) as NULL in SQL.

---

# 23. Indexes

An index helps Oracle locate rows more efficiently. Indexes consume storage and can add overhead to INSERT, UPDATE, and DELETE operations.

### CREATE INDEX

```sql
CREATE INDEX idx_salary
ON employees(salary);
```

### UNIQUE INDEX

```sql
CREATE UNIQUE INDEX idx_email
ON employees(email);
```

### Composite Index

```sql
CREATE INDEX idx_dept_salary
ON employees(department_id, salary);
```

The order of columns in a composite index matters. It is generally most useful for queries that filter on the leading column or leading column combination.

### DROP INDEX

Oracle syntax does not use `ON table_name` when dropping an index.

```sql
DROP INDEX idx_salary;
```

### View Indexes

```sql
SELECT index_name, table_name, uniqueness
FROM user_indexes
WHERE table_name = 'EMPLOYEES';
```

### View Indexed Columns

```sql
SELECT index_name, column_name, column_position
FROM user_ind_columns
WHERE table_name = 'EMPLOYEES'
ORDER BY index_name, column_position;
```

### Common Index Types

| Index | Meaning |
|---|---|
| B-tree | General-purpose index, common default |
| Unique | Enforces uniqueness through an index |
| Composite | Index on multiple columns |
| Bitmap | Can be useful for low-cardinality columns in suitable workloads |
| Function-based | Indexes the result of an expression |
| Reverse key | Reverses indexed key bytes to reduce certain index block contention patterns |

### Function-Based Index

```sql
CREATE INDEX idx_upper_name
ON employees(UPPER(first_name));
```

Can help queries that use the same expression:

```sql
SELECT *
FROM employees
WHERE UPPER(first_name) = 'PIYUSH';
```

---

# 24. Views

A view is a named query that behaves like a virtual table.

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

```sql
CREATE OR REPLACE VIEW employee_details AS
SELECT first_name, department_id
FROM employees;
```

### DROP VIEW

```sql
DROP VIEW employee_details;
```

### View Metadata

```sql
SELECT view_name
FROM user_views;
```

### Why Use Views?

- Simplifies complex queries.
- Reuses query logic.
- Can expose only selected rows or columns.
- Can provide a controlled interface to underlying tables.

**Note:** Not every view is directly updatable. Complex views may require an INSTEAD OF trigger for DML.

---

# 25. Sequences and Identity Columns

Oracle commonly uses sequences to generate numeric identifiers.

## CREATE SEQUENCE

```sql
CREATE SEQUENCE employee_seq
START WITH 1
INCREMENT BY 1
NOCACHE;
```

### NEXTVAL

Generates the next sequence value.

```sql
SELECT employee_seq.NEXTVAL
FROM dual;
```

### CURRVAL

Returns the current sequence value in the session, after NEXTVAL has been called in that session.

```sql
SELECT employee_seq.CURRVAL
FROM dual;
```

### Use a Sequence in INSERT

```sql
INSERT INTO employees (employee_id, first_name)
VALUES (employee_seq.NEXTVAL, 'Piyush');
```

### DROP SEQUENCE

```sql
DROP SEQUENCE employee_seq;
```

## Identity Column

Oracle 12c and later support identity columns.

```sql
CREATE TABLE students (
    student_id NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    student_name VARCHAR2(50)
);
```

Another option:

```sql
CREATE TABLE students (
    student_id NUMBER GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
    student_name VARCHAR2(50)
);
```

- `GENERATED ALWAYS`: Oracle generates the value; explicit values are normally not allowed.
- `GENERATED BY DEFAULT`: Oracle generates a value when one is not supplied; explicit values can be supplied.

---

# 26. PL/SQL Basics

PL/SQL stands for **Procedural Language extension to SQL**. It adds variables, conditions, loops, exceptions, and procedural programming to SQL.

### Basic PL/SQL Block

```sql
SET SERVEROUTPUT ON;

DECLARE
    v_name VARCHAR2(50) := 'Piyush';
BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello ' || v_name);
END;
/
```

A PL/SQL block contains:

| Section | Purpose |
|---|---|
| DECLARE | Optional variable declarations |
| BEGIN | Starts executable statements |
| EXCEPTION | Optional error handling |
| END | Ends the block |

The `/` on a separate line tells SQL Developer's script runner to execute the PL/SQL block.

### Variables

```sql
DECLARE
    v_salary NUMBER(10,2) := 5000;
    v_name VARCHAR2(50) := 'Piyush';
BEGIN
    DBMS_OUTPUT.PUT_LINE(v_name);
    DBMS_OUTPUT.PUT_LINE(v_salary);
END;
/
```

### %TYPE

Declares a variable using the data type of an existing table column.

```sql
DECLARE
    v_salary employees.salary%TYPE;
BEGIN
    SELECT salary
    INTO v_salary
    FROM employees
    WHERE employee_id = 101;

    DBMS_OUTPUT.PUT_LINE(v_salary);
END;
/
```

### %ROWTYPE

Declares a record with the structure of a table row.

```sql
DECLARE
    v_emp employees%ROWTYPE;
BEGIN
    SELECT *
    INTO v_emp
    FROM employees
    WHERE employee_id = 101;

    DBMS_OUTPUT.PUT_LINE(v_emp.first_name);
END;
/
```

### IF-ELSIF-ELSE

```sql
DECLARE
    v_salary NUMBER := 8000;
BEGIN
    IF v_salary >= 10000 THEN
        DBMS_OUTPUT.PUT_LINE('High');
    ELSIF v_salary >= 5000 THEN
        DBMS_OUTPUT.PUT_LINE('Medium');
    ELSE
        DBMS_OUTPUT.PUT_LINE('Low');
    END IF;
END;
/
```

### Simple LOOP

```sql
BEGIN
    FOR i IN 1..5 LOOP
        DBMS_OUTPUT.PUT_LINE(i);
    END LOOP;
END;
/
```

### WHILE LOOP

```sql
DECLARE
    v_counter NUMBER := 1;
BEGIN
    WHILE v_counter <= 5 LOOP
        DBMS_OUTPUT.PUT_LINE(v_counter);
        v_counter := v_counter + 1;
    END LOOP;
END;
/
```

### Basic Exception Handling

```sql
BEGIN
    UPDATE employees
    SET salary = salary * 1.1
    WHERE employee_id = 101;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No record found');
    WHEN TOO_MANY_ROWS THEN
        DBMS_OUTPUT.PUT_LINE('More than one record found');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE(SQLERRM);
END;
/
```

`NO_DATA_FOUND` and `TOO_MANY_ROWS` are especially relevant to SELECT INTO statements. An UPDATE statement that affects zero rows does not raise NO_DATA_FOUND.

---

# 27. Stored Procedures

A procedure is a named PL/SQL program stored in the database.

### CREATE PROCEDURE

```sql
CREATE OR REPLACE PROCEDURE get_employee (
    p_emp_id IN employees.employee_id%TYPE
)
AS
    v_name employees.first_name%TYPE;
BEGIN
    SELECT first_name
    INTO v_name
    FROM employees
    WHERE employee_id = p_emp_id;

    DBMS_OUTPUT.PUT_LINE('Employee: ' || v_name);

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Employee not found');
END;
/
```

### Execute Procedure

```sql
BEGIN
    get_employee(101);
END;
/
```

Or in SQL Developer:

```sql
EXEC get_employee(101);
```

### Procedure Parameters

| Parameter | Meaning |
|---|---|
| IN | Passes a value into the procedure |
| OUT | Returns a value to the caller |
| IN OUT | Accepts a value and can return a modified value |

### Procedure with OUT Parameter

```sql
CREATE OR REPLACE PROCEDURE get_employee_count (
    p_total OUT NUMBER
)
AS
BEGIN
    SELECT COUNT(*)
    INTO p_total
    FROM employees;
END;
/
```

Execute in a PL/SQL block:

```sql
DECLARE
    v_total NUMBER;
BEGIN
    get_employee_count(v_total);
    DBMS_OUTPUT.PUT_LINE(v_total);
END;
/
```

### DROP PROCEDURE

```sql
DROP PROCEDURE get_employee;
```

---

# 28. Functions

A function is a stored PL/SQL program that returns a value.

### CREATE FUNCTION

```sql
CREATE OR REPLACE FUNCTION annual_salary (
    p_monthly_salary NUMBER
)
RETURN NUMBER
IS
BEGIN
    RETURN p_monthly_salary * 12;
END;
/
```

### Execute Function in SQL

```sql
SELECT annual_salary(5000)
FROM dual;
```

### Function vs Procedure

| Function | Procedure |
|---|---|
| Must return a value | Does not have to return a value |
| Can be used in SQL expressions subject to SQL-callable restrictions | Called as a PL/SQL statement |
| Uses RETURN to provide a value | Can return values through OUT parameters |
| Useful for reusable calculations | Useful for performing operations |

---

# 29. Cursors

A cursor is a pointer to the result set of a SQL statement.

## Implicit Cursor

Oracle automatically creates an implicit cursor for SQL statements such as INSERT, UPDATE, DELETE, and SELECT INTO.

Useful attributes:

- `SQL%ROWCOUNT` — number of rows affected or returned by the most recent SQL statement.
- `SQL%FOUND` — TRUE if the most recent SQL statement affected or returned rows.
- `SQL%NOTFOUND` — TRUE if it did not affect or return rows.
- `SQL%ISOPEN` — always FALSE after an implicit SQL statement completes.

```sql
BEGIN
    UPDATE employees
    SET salary = salary * 1.1
    WHERE department_id = 10;

    DBMS_OUTPUT.PUT_LINE(SQL%ROWCOUNT);
END;
/
```

## Explicit Cursor

A programmer-defined cursor for processing a query result row by row.

```sql
DECLARE
    CURSOR emp_cursor IS
        SELECT employee_id, first_name
        FROM employees;

    v_id employees.employee_id%TYPE;
    v_name employees.first_name%TYPE;
BEGIN
    OPEN emp_cursor;

    LOOP
        FETCH emp_cursor INTO v_id, v_name;
        EXIT WHEN emp_cursor%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(v_id || ' ' || v_name);
    END LOOP;

    CLOSE emp_cursor;
END;
/
```

## Cursor FOR LOOP

Oracle automatically opens, fetches, and closes the cursor.

```sql
BEGIN
    FOR emp IN (
        SELECT employee_id, first_name
        FROM employees
    ) LOOP
        DBMS_OUTPUT.PUT_LINE(
            emp.employee_id || ' ' || emp.first_name
        );
    END LOOP;
END;
/
```

---

# 30. Triggers

A trigger automatically executes when a specified database event occurs.

### BEFORE INSERT Trigger

```sql
CREATE OR REPLACE TRIGGER before_employee_insert
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    IF :NEW.salary IS NULL THEN
        :NEW.salary := 0;
    END IF;
END;
/
```

### AFTER INSERT Trigger

```sql
CREATE OR REPLACE TRIGGER after_employee_insert
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
    DBMS_OUTPUT.PUT_LINE('New employee inserted');
END;
/
```

### Trigger Events

| Event | Meaning |
|---|---|
| BEFORE INSERT | Executes before a row is inserted |
| AFTER INSERT | Executes after a row is inserted |
| BEFORE UPDATE | Executes before a row is updated |
| AFTER UPDATE | Executes after a row is updated |
| BEFORE DELETE | Executes before a row is deleted |
| AFTER DELETE | Executes after a row is deleted |

### :NEW and :OLD

| Reference | Meaning |
|---|---|
| :NEW | New column values |
| :OLD | Existing column values before the change |

For INSERT, `:NEW` is available. For DELETE, `:OLD` is available. For UPDATE, both are available.

### DROP TRIGGER

```sql
DROP TRIGGER before_employee_insert;
```

---

# 31. Transactions

A transaction is a logical unit of work.

Oracle transactions begin implicitly when a transaction starts with a DML statement or an explicit transaction control statement.

### COMMIT

Permanently saves the current transaction.

```sql
UPDATE employees
SET salary = 10000
WHERE employee_id = 101;

COMMIT;
```

### ROLLBACK

Reverts uncommitted changes.

```sql
UPDATE employees
SET salary = 12000
WHERE employee_id = 101;

ROLLBACK;
```

### SAVEPOINT

Creates a point within a transaction to which you can roll back.

```sql
UPDATE employees
SET salary = 10000
WHERE employee_id = 101;

SAVEPOINT sp1;

UPDATE employees
SET salary = 12000
WHERE employee_id = 102;

ROLLBACK TO sp1;

COMMIT;
```

### ACID Properties

| Property | Meaning |
|---|---|
| Atomicity | All operations succeed or the transaction is rolled back |
| Consistency | Data remains valid according to database rules |
| Isolation | Concurrent transactions are controlled |
| Durability | Committed changes persist |

### Important Oracle Transaction Note

- DML changes can be committed or rolled back.
- DDL statements generally perform an implicit commit.
- `TRUNCATE` cannot be rolled back like an uncommitted DELETE.
- SQL Developer may have Auto Commit settings; check them when practicing transactions.

---

# 32. Performance and Execution Plans

### EXPLAIN PLAN

Shows the optimizer's estimated execution plan.

```sql
EXPLAIN PLAN FOR
SELECT *
FROM employees
WHERE salary > 10000;
```

Display the plan:

```sql
SELECT *
FROM TABLE(DBMS_XPLAN.DISPLAY);
```

### Gather Table Statistics

For suitable practice or administrative environments, statistics can be gathered using:

```sql
BEGIN
    DBMS_STATS.GATHER_TABLE_STATS(
        ownname => USER,
        tabname => 'EMPLOYEES'
    );
END;
/
```

This requires appropriate privileges and should be used thoughtfully in production.

### View Execution Plan in SQL Developer

1. Write your SELECT query.
2. Select the query.
3. Use the Explain Plan option or press F10.
4. Inspect the execution plan.

### Optimization Techniques

1. Avoid SELECT * when only selected columns are needed.
2. Index frequently filtered and joined columns when appropriate.
3. Avoid unnecessary DISTINCT operations.
4. Use appropriate data types.
5. Avoid unnecessary functions on indexed columns in filter conditions.
6. Use bind variables for repeated queries where appropriate.
7. Check execution plans.
8. Avoid unnecessary indexes.
9. Filter data as early as appropriate.
10. Keep optimizer statistics current.

---

# 33. SQL Logical Processing Order

The logical processing order helps explain why some aliases and aggregate expressions cannot be used in certain clauses.

A simplified logical order is:

| Order | Clause | Purpose |
|---|---|---|
| 1 | FROM | Identifies source tables |
| 2 | JOIN / ON | Combines rows |
| 3 | WHERE | Filters rows |
| 4 | GROUP BY | Groups rows |
| 5 | HAVING | Filters groups |
| 6 | SELECT | Calculates selected expressions |
| 7 | DISTINCT | Removes duplicate result rows |
| 8 | ORDER BY | Sorts the result |
| 9 | OFFSET / FETCH | Limits returned rows |

### Example

```sql
SELECT department_id, AVG(salary) AS avg_salary
FROM employees
WHERE salary > 3000
GROUP BY department_id
HAVING AVG(salary) > 8000
ORDER BY avg_salary DESC
FETCH FIRST 5 ROWS ONLY;
```

**Logical execution:**

1. FROM employees
2. WHERE salary > 3000
3. GROUP BY department_id
4. HAVING AVG(salary) > 8000
5. SELECT department_id and AVG(salary)
6. ORDER BY avg_salary DESC
7. FETCH FIRST 5 ROWS ONLY

**Interview tip:** A SELECT alias generally cannot be referenced in WHERE because WHERE is processed before SELECT. It can be referenced in ORDER BY.

---

# 34. Oracle SQL Interview Questions

Frequently asked SQL problems with Oracle-compatible solutions.

## Q1. Find the Highest Salary

```sql
SELECT MAX(salary)
FROM employees;
```

## Q2. Find the Second Highest Distinct Salary

```sql
SELECT MAX(salary)
FROM employees
WHERE salary < (
    SELECT MAX(salary)
    FROM employees
);
```

Alternative using DENSE_RANK:

```sql
SELECT salary
FROM (
    SELECT
        salary,
        DENSE_RANK() OVER(ORDER BY salary DESC) AS rnk
    FROM employees
)
WHERE rnk = 2
GROUP BY salary;
```

## Q3. Find the Nth Highest Salary

```sql
SELECT salary
FROM (
    SELECT
        salary,
        DENSE_RANK() OVER(ORDER BY salary DESC) AS rnk
    FROM employees
)
WHERE rnk = 3
GROUP BY salary;
```

Replace 3 with the required rank.

## Q4. Find Employees Earning More Than Company Average

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
)
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

## Q8. Find Duplicate Emails

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

## Q11. Find Employees Without a Matching Department

```sql
SELECT e.*
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

## Q12. Find Top 3 Salaries in Each Department

```sql
SELECT *
FROM (
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
WHERE hire_date >= TRUNC(SYSDATE, 'YYYY')
AND hire_date < ADD_MONTHS(TRUNC(SYSDATE, 'YYYY'), 12);
```

## Q15. Find the Difference Between Maximum and Minimum Salary

```sql
SELECT MAX(salary) - MIN(salary) AS salary_difference
FROM employees;
```

## Q16. Find Employees Who Share the Same Salary

```sql
SELECT salary, COUNT(*) AS employee_count
FROM employees
GROUP BY salary
HAVING COUNT(*) > 1;
```

## Q17. Find Departments Without Employees

```sql
SELECT d.*
FROM departments d
WHERE NOT EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

## Q18. Display Employees and Their Rank Within Each Department

```sql
SELECT
    first_name,
    department_id,
    salary,
    RANK() OVER(
        PARTITION BY department_id
        ORDER BY salary DESC
    ) AS dept_rank
FROM employees;
```

---

# 35. SQL Learning Roadmap

A structured roadmap for learning Oracle SQL from beginner to advanced.

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
- [ ] FETCH FIRST / OFFSET
- [ ] Column Aliases
- [ ] Aggregate Functions

### Intermediate

- [ ] GROUP BY
- [ ] HAVING
- [ ] CASE Expressions
- [ ] INNER JOIN
- [ ] LEFT JOIN
- [ ] RIGHT JOIN
- [ ] FULL OUTER JOIN
- [ ] CROSS JOIN
- [ ] SELF JOIN
- [ ] Multiple JOINs
- [ ] UNION / UNION ALL
- [ ] INTERSECT / MINUS
- [ ] Single-Row Subqueries
- [ ] Multiple-Row Subqueries
- [ ] Correlated Subqueries
- [ ] EXISTS / NOT EXISTS
- [ ] ANY / ALL

### Advanced SQL

- [ ] Common Table Expressions
- [ ] Recursive Subquery Factoring
- [ ] Hierarchical Queries
- [ ] Analytic Functions
- [ ] ROW_NUMBER()
- [ ] RANK()
- [ ] DENSE_RANK()
- [ ] NTILE()
- [ ] LAG() / LEAD()
- [ ] FIRST_VALUE() / LAST_VALUE()
- [ ] Running Totals
- [ ] Conditional Aggregation

### PL/SQL and Database Development

- [ ] PL/SQL Blocks
- [ ] Variables and Constants
- [ ] IF / ELSIF / ELSE
- [ ] Loops
- [ ] Exception Handling
- [ ] Procedures
- [ ] Functions
- [ ] Implicit and Explicit Cursors
- [ ] Triggers
- [ ] Sequences
- [ ] Identity Columns
- [ ] Views
- [ ] Indexes
- [ ] Transactions
- [ ] ACID Properties

### Performance

- [ ] EXPLAIN PLAN
- [ ] DBMS_XPLAN
- [ ] Index Optimization
- [ ] Query Optimization
- [ ] Execution Plan Analysis
- [ ] Bind Variables
- [ ] Table Statistics

---

# 📚 Additional Learning Resources

- [Oracle SQL Language Reference](https://docs.oracle.com/en/database/oracle/oracle-database/)
- [Oracle Database SQL Developer Documentation](https://docs.oracle.com/en/database/oracle/sql-developer/)
- [Oracle Live SQL](https://livesql.oracle.com/)
- [Oracle SQL Tutorial](https://www.oracletutorial.com/)
- [LeetCode SQL 50](https://leetcode.com/studyplan/top-sql-50/)
- [HackerRank SQL Practice](https://www.hackerrank.com/domains/sql)

---

## 💡 Final Note

SQL is best learned through consistent practice. Focus on understanding query logic instead of memorizing syntax.

For Oracle SQL interviews, pay particular attention to:

- JOINs and subqueries
- GROUP BY and HAVING
- Correlated subqueries
- EXISTS and NOT EXISTS
- CTEs
- Analytic and ranking functions
- NULL handling
- Oracle date functions
- PL/SQL procedures, functions, cursors, and triggers

**Happy Querying! 🚀**
