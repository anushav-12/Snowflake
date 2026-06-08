# Snowflake Views - Complete Guide

## What is a View?

A **View** is a virtual table created from a SQL query.

Unlike a table, a standard view does not store actual data. It stores only the query definition and metadata. Whenever the view is queried, Snowflake executes the underlying SQL and returns the latest data from the base tables.

### Example

```sql
CREATE VIEW emp_view AS
SELECT
    emp_id,
    emp_name,
    salary
FROM employees;
```

Querying the view:

```sql
SELECT * FROM emp_view;
```

Snowflake internally executes:

```sql
SELECT
    emp_id,
    emp_name,
    salary
FROM employees;
```

---

## Why Use Views?

Views are commonly used for:

* Simplifying complex SQL queries
* Restricting access to sensitive data
* Creating reusable business logic
* Building reporting layers
* Improving maintainability

### Example

Base Table:

| emp_id | emp_name | salary | ssn |
| ------ | -------- | ------ | --- |
| 101    | John     | 50000  | XXX |
| 102    | Alice    | 60000  | XXX |

Public View:

```sql
CREATE VIEW employee_public AS
SELECT
    emp_id,
    emp_name,
    salary
FROM employees;
```

Users can access salary information without seeing SSN data.

---

# Types of Views in Snowflake

## 1. Standard View

A Standard View stores only the SQL definition.

### Example

```sql
CREATE VIEW sales_view AS
SELECT *
FROM sales
WHERE amount > 1000;
```

### Characteristics

| Feature                  | Standard View |
| ------------------------ | ------------- |
| Stores Data              | No            |
| Stores Metadata          | Yes           |
| Additional Storage       | Minimal       |
| Returns Latest Data      | Yes           |
| Query Executes Each Time | Yes           |

### Advantages

* Easy to create
* Improves security
* Simplifies reporting

### Disadvantages

* Query executes every time
* Complex views may impact performance

---

## 2. Secure View

A Secure View provides enhanced security and hides implementation details.

### Example

```sql
CREATE SECURE VIEW employee_secure AS
SELECT
    emp_id,
    emp_name,
    salary
FROM employees;
```

### Benefits

* Hides underlying query logic
* Protects metadata
* Ideal for data sharing
* Improves security

### Common Use Cases

* External customer access
* Snowflake Data Sharing
* Sensitive business information

---

## 3. Materialized View

A Materialized View physically stores precomputed query results.

### Example

```sql
CREATE MATERIALIZED VIEW mv_sales AS
SELECT
    product_id,
    SUM(amount) AS total_sales
FROM sales
GROUP BY product_id;
```

### Characteristics

| Feature            | Materialized View |
| ------------------ | ----------------- |
| Stores Data        | Yes               |
| Stores Metadata    | Yes               |
| Additional Storage | Yes               |
| Faster Queries     | Yes               |
| Auto Maintenance   | Yes               |

### Advantages

* Faster query performance
* Efficient aggregations
* Great for dashboards

### Disadvantages

* Consumes storage
* Additional maintenance cost

---

## Materialized View Refresh in Snowflake

One major advantage of Snowflake is that Materialized Views are automatically maintained.

### Example

Base Table

| product_id | amount |
| ---------- | ------ |
| 101        | 100    |
| 101        | 200    |

Materialized View

| product_id | total_sales |
| ---------- | ----------- |
| 101        | 300         |

Insert new data:

```sql
INSERT INTO sales VALUES (101, 50);
```

Snowflake automatically updates the Materialized View.

Updated Result:

| product_id | total_sales |
| ---------- | ----------- |
| 101        | 350         |

### Important

Snowflake does **not** require:

```sql
REFRESH MATERIALIZED VIEW mv_sales;
```

Materialized Views are automatically maintained by Snowflake.

---

## Materialized Views in Other Databases

### PostgreSQL

Requires manual refresh:

```sql
REFRESH MATERIALIZED VIEW mv_sales;
```

### Oracle

Common refresh methods:

```sql
EXEC DBMS_MVIEW.REFRESH('MV_SALES');
```

Refresh options:

* ON COMMIT
* ON DEMAND
* Scheduled Refresh

### MySQL

No native Materialized View support.

### SQL Server

Uses Indexed Views instead of Materialized Views.

---

## 4. Recursive View

Used for hierarchical data structures.

### Example Hierarchy

```text
CEO
├── Manager1
│   ├── Employee1
│   └── Employee2
└── Manager2
    └── Employee3
```

### Example

```sql
CREATE RECURSIVE VIEW org_hierarchy AS
WITH RECURSIVE cte AS
(
    SELECT
        emp_id,
        manager_id,
        emp_name
    FROM employees
    WHERE manager_id IS NULL

    UNION ALL

    SELECT
        e.emp_id,
        e.manager_id,
        e.emp_name
    FROM employees e
    JOIN cte
      ON e.manager_id = cte.emp_id
)
SELECT *
FROM cte;
```

### Use Cases

* Organization structures
* Folder hierarchies
* Product categories
* Bill of Materials (BOM)

---

# View vs Materialized View

| Feature               | View                   | Materialized View |
| --------------------- | ---------------------- | ----------------- |
| Stores Data           | No                     | Yes               |
| Stores Metadata       | Yes                    | Yes               |
| Query Speed           | Normal                 | Faster            |
| Storage Cost          | Low                    | Higher            |
| Query Recalculation   | Every Query            | Precomputed       |
| Automatic Maintenance | N/A                    | Yes               |
| Best Use Case         | Security & Abstraction | Performance       |

---

# What Does "Stores Only Metadata" Mean?

Consider:

```sql
CREATE VIEW emp_view AS
SELECT
    emp_id,
    emp_name
FROM employees;
```

Snowflake stores:

* View Name
* Schema Information
* Owner Information
* Creation Date
* Column Definitions
* SQL Definition

Snowflake does **not** store:

* Employee rows
* Table data
* Query results

Actual data remains in the base table.

---

# What Happens If the Base Table Changes?

### Add a Column

```sql
ALTER TABLE employees
ADD department VARCHAR;
```

View continues to work.

### Drop a Referenced Column

```sql
ALTER TABLE employees
DROP COLUMN emp_name;
```

View breaks.

### Rename a Referenced Column

```sql
ALTER TABLE employees
RENAME COLUMN emp_name TO employee_name;
```

View breaks.

### Drop the Table

```sql
DROP TABLE employees;
```

View remains in metadata but becomes unusable.

---

# Nested Views

A view can be created from another view.

### Example

```sql
CREATE VIEW v_emp AS
SELECT *
FROM employees;
```

```sql
CREATE VIEW v_high_salary AS
SELECT *
FROM v_emp
WHERE salary > 100000;
```

### Advantages

* Reusable logic
* Modular architecture

### Disadvantages

* Harder debugging
* More complex maintenance

---

# Security Using Views

Instead of granting access directly to tables:

```sql
GRANT SELECT ON TABLE employees TO ROLE analyst;
```

Organizations often grant access to views:

```sql
GRANT SELECT ON VIEW employee_public TO ROLE analyst;
```

Benefits:

* Better security
* Controlled data exposure
* Hides sensitive columns

---

# Useful Snowflake Commands

## Show All Views

```sql
SHOW VIEWS;
```

---

## Describe a View

```sql
DESC VIEW emp_view;
```

or

```sql
DESCRIBE VIEW emp_view;
```

Returns:

* Column names
* Data types
* Metadata information

---

## Get View Definition

```sql
SELECT GET_DDL('VIEW', 'EMP_VIEW');
```

Returns the original CREATE VIEW statement.

Useful for:

* Debugging
* Migration
* Reverse engineering

---

# Important

### What is a View?

A virtual table that stores query definitions and metadata rather than actual data.

### What is a Materialized View?

A database object that physically stores query results for faster retrieval.

### Does Snowflake Materialized View Require Manual Refresh?

No. Snowflake automatically maintains Materialized Views when base tables change.

### Difference Between View and Materialized View?

| View                     | Materialized View       |
| ------------------------ | ----------------------- |
| Stores Query Definition  | Stores Query Results    |
| No Data Storage          | Physical Data Storage   |
| Query Executes Each Time | Faster Retrieval        |
| Minimal Storage Cost     | Additional Storage Cost |

### Why Use Secure Views?

To hide underlying implementation details and improve data security.

---

# Key Takeaways

* Views are virtual tables.
* Standard Views store metadata only.
* Secure Views provide enhanced security.
* Materialized Views store precomputed results.
* Snowflake automatically maintains Materialized Views.
* Recursive Views support hierarchical data.
* Views are widely used for security, abstraction, and reporting.
* Materialized Views are primarily used for performance optimization.
* `SHOW VIEWS`, `DESC VIEW`, and `GET_DDL()` are essential Snowflake commands.
