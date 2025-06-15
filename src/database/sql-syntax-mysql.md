# Data Types

| Category      | Data Type                                    | Description / Use Case                                                                |
|---------------|----------------------------------------------|---------------------------------------------------------------------------------------|
| **Numeric**   | `TINYINT`                                    | 1 byte, range: -128 to 127 or 0 to 255 (unsigned)                                     |
|               | `SMALLINT`                                   | 2 bytes, range: -32,768 to 32,767                                                     |
|               | `MEDIUMINT`                                  | 3 bytes, range: -8M to 8M                                                             |
|               | `INT` / `INTEGER`                            | 4 bytes, range: -2B to 2B                                                             |
|               | `BIGINT`                                     | 8 bytes, range: ±9 quintillion                                                        |
|               | `DECIMAL(M,D)`                               | Exact fixed-point (e.g. money); `M` = digits total, `D` = digits after decimal        |
|               | `FLOAT`, `DOUBLE`                            | Approximate floating-point numbers                                                    |
|               | `BIT(M)`                                     | Bit field (up to 64 bits); use for flags or binary values                             |
| **String**    | `CHAR(n)`                                    | Fixed-length string, max 255 chars. Padded with spaces if shorter                     |
|               | `VARCHAR(n)`                                 | Variable-length string, max 65,535 bytes (depending on row size & charset)            |
|               | `TEXT` types                                 | `TINYTEXT` (255), `TEXT` (64K), `MEDIUMTEXT` (16MB), `LONGTEXT` (4GB)                 |
|               | `BINARY(n)`                                  | Fixed-length binary data                                                              |
|               | `VARBINARY(n)`                               | Variable-length binary data                                                           |
|               | `BLOB` types                                 | Binary Large Objects: same sizes as TEXT types                                        |
|               | `ENUM(...)`                                  | String with a predefined list of values (e.g. ENUM('small', 'medium', 'large'))       |
|               | `SET(...)`                                   | String object with 0 or more values from a predefined list                            |
| **Date/Time** | `DATE`                                       | 'YYYY-MM-DD', range: 1000-01-01 to 9999-12-31                                         |
|               | `DATETIME`                                   | 'YYYY-MM-DD HH:MM:SS', fractional seconds optional                                    |
|               | `TIMESTAMP`                                  | Similar to `DATETIME`, auto-updated on insert/update, stored in UTC                   |
|               | `TIME`                                       | 'HH:MM:SS', range: -838:59:59 to 838:59:59                                            |
|               | `YEAR`                                       | Year in 4-digit format (1901 to 2155)                                                 |
| **Spatial**   | `GEOMETRY`, `POINT`, `LINESTRING`, `POLYGON` | For geospatial data (requires Spatial Extensions)                                     |
| **JSON**      | `JSON`                                       | Stores and validates JSON documents; indexed with generated columns or JSON functions |

---

## 📝 Notes

- Prefer `VARCHAR` over `CHAR` unless fixed-length (e.g. country codes).
- Use `DECIMAL` for money to avoid floating-point errors.
- Avoid using `ENUM`/`SET` if the values are likely to change.
- Use `TIMESTAMP` for auto-updated time fields (e.g. `updated_at`).
- Know size implications: e.g., `TEXT` can't be indexed fully without a prefix length.
- `NULL` vs `NOT NULL`: affects indexing and query planning.

# CREATE TABLE Syntax

## Basic Syntax

```sql
CREATE TABLE employees
(
    id         INT         NOT NULL AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name  VARCHAR(50),
    email      VARCHAR(100) UNIQUE,
    salary     DECIMAL(10, 2) DEFAULT 0.00,
    status     ENUM('active', 'inactive') NOT NULL DEFAULT 'active',
    hired_at   DATE,
    updated_at TIMESTAMP      DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    PRIMARY KEY (id)
);
```

## Notes

- DEFAULT CURRENT_TIMESTAMP: When a row is inserted, this column will automatically get the current time
- ON UPDATE CURRENT_TIMESTAMP: When a row is updated, this column will automatically update to the new current time
- AUTO_INCREMENT requires column to be part of a PRIMARY KEY or UNIQUE index to enforce uniqueness
- Always include constraints like NOT NULL, UNIQUE, or DEFAULT explicitly for clarity.
- Use DECIMAL over FLOAT/DOUBLE when precision matters (e.g., salaries).

## Table Constraints

```sql
CREATE TABLE employees
(
    emp_id        INT AUTO_INCREMENT,
    email         VARCHAR(100) NOT NULL,
    department_id INT,
    salary        DECIMAL(10, 2) CHECK (salary > 0),
    hired_at      DATE DEFAULT CURRENT_DATE,

    PRIMARY KEY (emp_id),
    UNIQUE (email),
    FOREIGN KEY (department_id) REFERENCES departments (dept_id) ON DELETE SET NULL
);
```

| Constraint           | Code Snippet                                                  | Meaning                                                      |
|----------------------|---------------------------------------------------------------|--------------------------------------------------------------|
| `PRIMARY KEY`        | `PRIMARY KEY (emp_id)`                                        | Ensures `emp_id` is unique and not null (only one per table) |
| `UNIQUE`             | `UNIQUE (email)`                                              | No two employees can share the same email                    |
| `FOREIGN KEY`        | `FOREIGN KEY (department_id) REFERENCES departments(dept_id)` | Links to another table; ensures valid dept ID                |
| `ON DELETE SET NULL` | When department is deleted, `department_id` becomes `NULL`    | Maintains referential integrity                              |
| `CHECK`              | `CHECK (salary > 0)` *(MySQL 8.0+)*                           | Prevents negative/zero salaries                              |
| `DEFAULT`            | `DEFAULT CURRENT_DATE`                                        | Automatically sets today’s date if not provided              |

## CONSTRAINT keyword

The CONSTRAINT keyword is used to name a constraint when defining it. It make it easier to reference in ALTER or DROP
statements & error messages mention constraint names.

```sql
CREATE TABLE employees
(
    id      INT NOT NULL,
    email   VARCHAR(100),
    dept_id INT,
    salary  DECIMAL(10, 2),

    CONSTRAINT pk_emp PRIMARY KEY (id),
    CONSTRAINT uq_email UNIQUE (email),
    CONSTRAINT fk_dept FOREIGN KEY (dept_id) REFERENCES departments (id),
    CONSTRAINT chk_salary CHECK (salary > 0)
);
```


# INSERT syntax

```sql
INSERT INTO table_name (column1, column2, ...) 
VALUES (value1, value2, ...);
```

```sql
INSERT INTO table_name 
VALUES (value1, value2, ...);
```

```sql
INSERT INTO employees (id, name)
VALUES 
  (3, 'Charlie'),
  (4, 'Diana');
```

```sql
INSERT INTO employees (name) VALUES ('Eve');
-- Other columns use their DEFAULT or NULL
```

# UPDATE syntax

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

```sql
-- Update single row
UPDATE employees
SET salary = 6000, department = 'HR'
WHERE id = 3;
```

```sql
-- Update multiple rows
UPDATE employees
SET department = 'Sales'
WHERE department = 'Marketing';
```

```sql
-- Update all rows
UPDATE employees
SET bonus = 1000;
```

```sql
-- Update with subquery
UPDATE employees
SET department = (
  SELECT default_dept FROM settings WHERE role = 'junior'
)
WHERE role = 'junior';
```

# DELETE syntax

```sql
DELETE FROM table_name
WHERE condition;
```

```sql
DELETE FROM employees
WHERE department = 'HR' AND salary < 4000;
```

# TRUNCATE syntax

- No WHERE clause allowed – use DELETE if you need selective row deletion.
- Considered a DDL operation, not DML.
- In MySQL with InnoDB, TRUNCATE is effectively a DROP + CREATE.

```sql
TRUNCATE TABLE employees;
```

# ALTER TABLE syntax

```sql
-- Add column
    
-- ALTER TABLE table_name
-- ADD COLUMN column_name datatype [constraints];

ALTER TABLE users ADD age INT DEFAULT 18;
```

```sql
-- Modify column

-- ALTER TABLE table_name
-- MODIFY COLUMN column_name new_datatype [constraints];

ALTER TABLE users MODIFY age SMALLINT NOT NULL;
```

```sql
-- Change Column (rename + type)

-- ALTER TABLE table_name
-- CHANGE COLUMN old_name new_name datatype [constraints];

ALTER TABLE users CHANGE age user_age INT;       
```

```sql
-- Drop Column

-- ALTER TABLE table_name
-- DROP COLUMN column_name;

ALTER TABLE users DROP COLUMN temp_flag;
```

```sql
-- Add Primary Key
ALTER TABLE employees ADD CONSTRAINT pk_emp PRIMARY KEY (id);
```

```sql
-- Drop Foreign Key
ALTER TABLE employees DROP FOREIGN KEY fk_dept;
```

```sql
-- Drop Index
ALTER TABLE users DROP INDEX uq_email;
```

```sql
-- Combine Multiple Changes
ALTER TABLE users
ADD COLUMN is_active BOOLEAN DEFAULT TRUE,
MODIFY COLUMN email VARCHAR(150) NOT NULL;
```

# Query Clauses

| Clause name | Purpose                                                                   |
|-------------|---------------------------------------------------------------------------|
| `select`    | Determines which columns to include in the query’s result set             |
| `from`      | Identifies the tables from which to retrieve data and how they are joined |
| `where`     | Filters out unwanted data                                                 |
| `group by`  | Groups rows together by common column values                              |
| `having`    | Filters out unwanted groups (after grouping)                              |
| `order by`  | Sorts the rows of the final result set by one or more columns             |

## Execution Order

1. FROM 
2. WHERE 
3. GROUP BY 
4. HAVING 
5. SELECT 
6. ORDER BY 
7. LIMIT

# SELECT Query Clause

```sql
SELECT column1, column2, ...
FROM table_name
[WHERE ...]
[GROUP BY ...]
[HAVING ...]
[ORDER BY ...];
```

```sql
-- Select specific columns
SELECT id, name FROM users;

-- Select all columns
SELECT * FROM users;

-- Use expressions or functions
SELECT id, UPPER(name), salary * 1.1 AS adjusted_salary FROM employees;

-- Use DISTINCT to eliminate duplicates
SELECT DISTINCT department FROM employees;
```

## Notes

- SELECT is the first visible, but last executed
- Avoid SELECT * in real systems: May expose sensitive data, hurts performance
- Use aliases (AS) for calculated fields: readability
- Use LIMIT for paging/debugging: Prevents returning massive result sets
- SELECT COUNT(*) vs SELECT COUNT(col): One counts rows, one skips NULLs

# Column Alias

```sql
-- With AS (recommended for clarity)
SELECT salary * 1.1 AS adjusted_salary FROM employees;

-- Without AS (valid but less readable)
SELECT salary * 1.1 adjusted_salary FROM employees;
```

# Table Types

1. Permanent Tables: Created using CREATE TABLE. Persist across sessions.

2. Derived Tables (Subqueries):

- Generated by subqueries in the FROM clause.
- Exist only in memory during query execution.
- Must be aliased to be referenced by the outer query.

```sql
SELECT ... FROM (SELECT ...) AS alias
```

3. Temporary Tables:

- Created using CREATE TEMPORARY TABLE.
- Volatile: data disappears after session ends.
- Useful for staging intermediate results.

4. Views (Virtual Tables):
- Created using CREATE VIEW.
- Store query definitions, not data.
- When queried, view definition merges with your query.

# GROUP BY Clause

Aggregates data by unique values in one or more columns.

```sql
SELECT department_id, COUNT(*) AS num_employees
FROM employees
GROUP BY department_id;
```

# HAVING Clause

Filters aggregated/grouped results (unlike WHERE, which filters rows before grouping).

```sql
SELECT department_id, COUNT(*) AS num_employees
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 10;
```

# ORDER BY Clause

Sorts the final result set.

```sql
SELECT department_id, COUNT(*) AS num_employees
FROM employees
GROUP BY department_id
ORDER BY num_employees DESC;
```

## WHERE vs HAVING

| Feature                   | `WHERE`                                   | `HAVING`                                              |
|---------------------------|-------------------------------------------|-------------------------------------------------------|
| **When it's evaluated**   | **Before** grouping (`GROUP BY`)          | **After** grouping and aggregation                    |
| **Filters**               | Individual rows                           | Grouped/aggregated results                            |
| **Used with aggregates?** | ❌ Cannot use aggregate functions directly | ✅ Can use aggregate functions like `SUM()`, `COUNT()` |
| **Performance**           | Faster, since it reduces rows early       | Slower if misused (works on grouped data)             |

# WHERE Clause

## Condition Evaluation Logic

```sql
-- AND Operator: ALL conditions must be true for row inclusion
WHERE first_name = 'STEVEN' AND create_date > '2006-01-01'
``` 

```sql
-- OR Operator: ANY condition can be true for row inclusion
WHERE first_name = 'STEVEN' OR create_date > '2006-01-01'
```

```sql
-- Parentheses: Use to clarify complex logic
WHERE (first_name = 'STEVEN' OR last_name = 'YOUNG') AND create_date > '2006-01-01'
```

```sql
-- NOT Operator: Negates conditions, but avoid when possible for readability
WHERE NOT (first_name = 'STEVEN')
```

## Condition Types

```sql
-- Equality/Inequality Conditions

-- Equality
WHERE title = 'RIVER OUTLAW'
WHERE film_id = (SELECT film_id FROM film WHERE title = 'RIVER OUTLAW')

-- Inequality  
WHERE rating <> 'PG-13'  -- or !=
```

```sql
-- Range Conditions

-- Basic range
WHERE rental_date >= '2005-06-14' AND rental_date <= '2005-06-16'

-- BETWEEN operator (inclusive)
WHERE rental_date BETWEEN '2005-06-14' AND '2005-06-16'
WHERE amount BETWEEN 10.0 AND 11.99
WHERE last_name BETWEEN 'FA' AND 'FR'  -- String ranges
```

```sql
-- Membership Conditions

-- IN operator
WHERE rating IN ('G', 'PG')
WHERE rating NOT IN ('PG-13', 'R', 'NC-17')

-- Subquery with IN
WHERE rating IN (SELECT rating FROM film WHERE title LIKE '%PET%')
```

```sql
-- Pattern Matching

-- LIKE with wildcards
WHERE last_name LIKE '_A_T%S'  -- A in 2nd position, T in 4th, ends with S
WHERE last_name LIKE 'Q%'      -- Starts with Q
WHERE last_name LIKE '%bas%'   -- Contains 'bas'

-- Regular expressions (MySQL)
WHERE last_name REGEXP '^[QY]'  -- Starts with Q or Y
```

## NULL Handling

- An expression can BE null, but never EQUALS null
- Two nulls are never equal to each other
- Always use IS NULL or IS NOT NULL

```sql
-- CORRECT
WHERE return_date IS NULL
WHERE return_date IS NOT NULL

-- WRONG - returns no results!
WHERE return_date = NULL

-- This misses NULL values!
WHERE return_date NOT BETWEEN '2005-05-01' AND '2005-09-01'

-- Correct approach includes NULLs
WHERE return_date IS NULL 
   OR return_date NOT BETWEEN '2005-05-01' AND '2005-09-01'
```

# Inner Join

## Basic Syntax

```sql
SELECT c.first_name, c.last_name, a.address
FROM customer c INNER JOIN address a
  ON c.address_id = a.address_id;
```

## Key Points

- Only returns rows where join condition is satisfied in BOTH tables
- If no match exists, row is excluded from result set
- Most commonly used join type
- INNER keyword is optional but recommended for clarity

## Alternative Syntax (USING):

```sql
-- When column names are identical
FROM customer c INNER JOIN address a
  USING (address_id);
```

## Subqueries as Tables

```sql
SELECT c.first_name, c.last_name, addr.address, addr.city
FROM customer c
  INNER JOIN (
    SELECT a.address_id, a.address, ct.city
    FROM address a INNER JOIN city ct ON a.city_id = ct.city_id
    WHERE a.district = 'California'
  ) addr ON c.address_id = addr.address_id;
```

# Join Algorithm

## Nested Loop Join

- For each row in the outer table, scan the entire inner table looking for matches
- Most basic but often inefficient algorithm
- When used: When no indexes are available
- O(n × m) where n and m are table sizes

```
FOR each row R1 in Table1:
    FOR each row R2 in Table2:
        IF R1.key = R2.key:
            OUTPUT (R1, R2)
```

## Index Nested Loop Join

- For each row in outer table, use an index to quickly find matching rows in inner table
- Much more efficient than basic nested loop
- When used: When inner table has an index on join column & Outer table is relatively small
- O(n × log m) with index


```sql
FOR each row R1 in Table1:
    Use index to find matching rows in Table2
    FOR each matching row R2:
        OUTPUT (R1, R2)
```

## Sort-Merge Join

- Sort both tables by join column(s)
- Merge the sorted results by scanning both tables simultaneously
- When used: Large tables, When data is already sorted, When no suitable indexes exist
- Performance: O(n log n + m log m) for sorting + O(n + m) for merging

```
Sort Table1 by join_key
Sort Table2 by join_key
WHILE not end of either table:
    IF Table1.key = Table2.key:
        OUTPUT matching rows
        Advance both pointers
    ELSE IF Table1.key < Table2.key:
        Advance Table1 pointer
    ELSE:
        Advance Table2 pointer
```

## Hash Join

- Build a hash table from the smaller table
- Probe the hash table with each row from the larger table
- Large tables with no useful indexes, When one table fits in memory

```
// Build phase
FOR each row R1 in smaller_table:
    hash_table[hash(R1.key)] = R1

// Probe phase  
FOR each row R2 in larger_table:
    IF hash_table[hash(R2.key)] exists:
        FOR each matching row:
            OUTPUT (matching_row, R2)
```

# Set Operations

```sql
-- UNION ALL - keeps duplicates
SELECT first_name, last_name FROM customer
WHERE first_name LIKE 'J%'
UNION ALL
SELECT first_name, last_name FROM actor
WHERE first_name LIKE 'J%';

-- UNION - removes duplicates
SELECT first_name, last_name FROM customer
WHERE first_name LIKE 'J%'
UNION
SELECT first_name, last_name FROM actor
WHERE first_name LIKE 'J%';

-- Find common names between tables
SELECT first_name, last_name FROM customer
WHERE first_name LIKE 'J%'
INTERSECT
SELECT first_name, last_name FROM actor
WHERE first_name LIKE 'J%';
-- Returns only: JENNIFER DAVIS (if exists in both)

-- Find names in actors but not in customers
SELECT first_name, last_name FROM actor
WHERE first_name LIKE 'J%'
EXCEPT
SELECT first_name, last_name FROM customer
WHERE first_name LIKE 'J%';
```

# GROUPING

## Basic GROUP BY syntax

```sql
SELECT customer_id, COUNT(*)
FROM rental
GROUP BY customer_id;
-- Returns one row per customer with rental count
```

## Common Aggregate Functions

- COUNT(*) - Number of rows in group
- COUNT(column) - Number of non-NULL values in column
- SUM(column) - Sum of values
- AVG(column) - Average of values
- MIN(column) - Minimum value
- MAX(column) - Maximum value

## GROUP BY Rules

- Every non-aggregate column in SELECT must be in GROUP BY
- Cannot use aggregate functions in WHERE clause
- Use HAVING for filtering grouped data

```sql
-- WRONG - Error: customer_id not in GROUP BY
SELECT customer_id, COUNT(*)
FROM payment;

-- CORRECT
SELECT customer_id, COUNT(*)
FROM payment
GROUP BY customer_id;
```

## HAVING Clause

```sql
SELECT customer_id, COUNT(*)
FROM rental
WHERE rental_date > '2005-01-01'  -- Filter before grouping
GROUP BY customer_id
HAVING COUNT(*) >= 40;            -- Filter after grouping
```

## Types of Grouping

```sql
SELECT actor_id, COUNT(*)
FROM film_actor
GROUP BY actor_id;
-- One group per actor

SELECT actor_id, rating, COUNT(*)
FROM film_actor fa
INNER JOIN film f ON fa.film_id = f.film_id
GROUP BY actor_id, rating;
-- One group per actor/rating combination

SELECT EXTRACT(YEAR FROM rental_date) year, COUNT(*)
FROM rental
GROUP BY EXTRACT(YEAR FROM rental_date);
-- Group by calculated values
```

# Subqueries

























