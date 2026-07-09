# Databases, Schemas & Tables in Snowflake

## Overview

Snowflake organizes data in a hierarchical structure:

```text
Account
   │
Database
   │
Schema
   │
Table
```

This structure helps organize and manage data efficiently.

---

# 1. Database

A **Database** is the top-level container that stores schemas and database objects.

### Purpose
- Organize related data
- Separate different business domains
- Manage access and security

### Example

```sql
CREATE DATABASE COMPANY_DB;
```

### Best Practices
- Use meaningful names.
- Create separate databases for different environments (DEV, TEST, PROD).

---

# 2. Schema

A **Schema** is a logical container inside a database that stores objects such as tables, views, procedures, and functions.

### Purpose
- Organize objects logically
- Improve maintainability
- Simplify permission management

### Example

```sql
CREATE SCHEMA HR;
```

### Example Structure

```text
COMPANY_DB
│
├── HR
├── SALES
├── FINANCE
└── MARKETING
```

---

# 3. Table

A **Table** stores data in rows and columns.

### Example

```sql
CREATE TABLE EMPLOYEES (
    EMP_ID INT,
    EMP_NAME STRING,
    DEPARTMENT STRING,
    SALARY NUMBER(10,2)
);
```

### Insert Data

```sql
INSERT INTO EMPLOYEES
VALUES
(1,'Anusha','IT',60000),
(2,'Rahul','HR',50000);
```

### Retrieve Data

```sql
SELECT *
FROM EMPLOYEES;
```

---

# Relationship

```text
Database
   │
   ├── Schema
   │      │
   │      ├── Table
   │      ├── View
   │      ├── Procedure
   │      └── Function
```

---

# PostgreSQL vs Snowflake

| PostgreSQL | Snowflake |
|------------|------------|
| Database | Database |
| Schema | Schema |
| Table | Table |
| Function | Procedure / Function |
| View | View |

The hierarchy is almost the same, making it easy for PostgreSQL developers to learn Snowflake.

---

# Best Practices

- Create separate databases for each application or environment.
- Group related objects into schemas.
- Use meaningful naming conventions.
- Avoid creating everything under the PUBLIC schema.
- Grant permissions at the schema level whenever possible.

---

# Interview Questions

### What is a Database?

A Database is the top-level container that stores schemas and database objects.

---

### What is a Schema?

A Schema is a logical container inside a database used to organize objects like tables, views, procedures, and functions.

---

### Can one database contain multiple schemas?

**Yes.**

Example:

```
COMPANY_DB
├── HR
├── SALES
├── FINANCE
```

---

### Can two schemas have tables with the same name?

**Yes.**

Example:

```
HR.EMPLOYEES

SALES.EMPLOYEES
```

Both tables can exist because they belong to different schemas.

---

### Why are Schemas used?

- Better organization
- Easier security management
- Avoid object name conflicts
- Improved maintainability

---

# Summary

- A **Database** is the highest-level container.
- A **Schema** organizes related database objects.
- A **Table** stores data in rows and columns.
- Snowflake follows the hierarchy:

```
Database
   │
Schema
   │
Table
```

- PostgreSQL and Snowflake have a very similar organizational structure.
