Cheat sheet
SQL Cheat Sheet
1. Database Management
Create, Modify, and Delete Tables
-- Create a new table with an integer primary key and a text column
CREATE TABLE table_name (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

-- Add a new column to an existing table
ALTER TABLE table_name ADD column_name INT;

-- Delete a table permanently
DROP TABLE table_name;
Indexes and Views
-- Create an index to speed up searches on a column
CREATE INDEX idx_name ON table_name (column1);

-- Remove an existing index
DROP INDEX idx_name;

-- Create a virtual table based on a query
CREATE VIEW view_name AS SELECT column1, column2 FROM table_name;

-- Remove a view
DROP VIEW view_name;
Renaming Tables
-- Rename a table
RENAME TABLE old_table TO new_table;
2. Data Querying
Basic Queries
-- Select specific columns from a table
SELECT column1, column2 FROM table_name;

-- Retrieve unique values from a column
SELECT DISTINCT column1 FROM table_name;
Filtering Data
-- Select rows that meet a specific condition
SELECT * FROM table_name WHERE column1 = 'value';

-- Filter rows with multiple possible values
SELECT * FROM table_name WHERE column1 IN ('value1', 'value2');

-- Filter rows within a range
SELECT * FROM table_name WHERE column1 BETWEEN 10 AND 50;

-- Search for patterns in text columns
SELECT * FROM table_name WHERE column1 LIKE 'A%';
Sorting and Limiting
-- Sort results in ascending order
SELECT * FROM table_name ORDER BY column1 ASC;

-- Limit the number of rows returned
SELECT * FROM table_name LIMIT 10;
3. Aggregation & Grouping
-- Count the number of rows
SELECT COUNT(*) FROM table_name;

-- Sum values of a numeric column
SELECT SUM(column1) FROM table_name;

-- Calculate the average of a numeric column
SELECT AVG(column1) FROM table_name;

-- Find the minimum value
SELECT MIN(column1) FROM table_name;

-- Find the maximum value
SELECT MAX(column1) FROM table_name;

-- Group rows for aggregate functions
SELECT column1, COUNT(*) FROM table_name GROUP BY column1;

-- Filter grouped results
SELECT column1, COUNT(*) FROM table_name GROUP BY column1 HAVING COUNT(*) > 5;
4. Data Manipulation
-- Insert new rows into a table
INSERT INTO table_name (column1, column2) VALUES ('value1', 'value2');

-- Update existing rows
UPDATE table_name SET column1 = 'new_value' WHERE id = 1;

-- Delete specific rows
DELETE FROM table_name WHERE column1 = 'value';

-- Remove all rows from a table quickly
TRUNCATE TABLE table_name;
5. Joins & Set Operations
-- Retrieve matching records from two tables
SELECT * FROM table1 INNER JOIN table2 ON table1.id = table2.id;

-- Retrieve all records from the left table and matching ones from the right table
SELECT * FROM table1 LEFT JOIN table2 ON table1.id = table2.id;

-- Retrieve all records from the right table and matching ones from the left table
SELECT * FROM table1 RIGHT JOIN table2 ON table1.id = table2.id;

-- Retrieve all records from both tables
SELECT * FROM table1 FULL OUTER JOIN table2 ON table1.id = table2.id;

-- Create a Cartesian product of both tables
SELECT * FROM table1 CROSS JOIN table2;
Set Operations
-- Combine results from two queries and remove duplicates
SELECT column1 FROM table1 UNION SELECT column1 FROM table2;

-- Combine results from two queries and include duplicates
SELECT column1 FROM table1 UNION ALL SELECT column1 FROM table2;
6. Window Functions
-- Assign a unique row number to each partition
SELECT column1, ROW_NUMBER() OVER (PARTITION BY column2 ORDER BY column3) FROM table_name;

-- Rank rows with gaps for ties
SELECT column1, RANK() OVER (PARTITION BY column2 ORDER BY column3) FROM table_name;
7. Subqueries & Common Table Expressions (CTEs)
-- Subquery to get maximum value
SELECT column1 FROM table1 WHERE column2 = (SELECT MAX(column2) FROM table2);

-- Using a Common Table Expression (CTE)
WITH CTE AS (
    SELECT column1, AVG(column2) AS avg_value FROM table_name GROUP BY column1
)
SELECT * FROM CTE WHERE avg_value > 50;
8. Transactions & Stored Procedures
-- Start a transaction and commit changes
START TRANSACTION;
UPDATE table_name SET column1 = 'value' WHERE id = 1;
COMMIT;

-- Rollback if needed
ROLLBACK;
Stored Procedures
-- Create a stored procedure
CREATE PROCEDURE GetEmployee(IN emp_id INT)
BEGIN
    SELECT * FROM employees WHERE id = emp_id;
END;

-- Execute a stored procedure
CALL GetEmployee(1);
9. String & Date Functions
-- Concatenate two strings
SELECT CONCAT(first_name, ' ', last_name) FROM employees;

-- Extract a substring
SELECT SUBSTRING(column1, 1, 5) FROM table_name;

-- Get the current timestamp
SELECT NOW();
10. Performance Optimization
-- Create an index for faster searches
CREATE INDEX idx_column ON table_name(column1);

-- Analyze a query execution plan
EXPLAIN SELECT * FROM table_name WHERE column1 > 1000;
