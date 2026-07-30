# ⏳ Snowflake Time Travel

## What is Time Travel?

Time Travel is a Snowflake feature that allows you to access and recover historical versions of data within a configured retention period.

### It is used to:
- Query historical data
- Recover deleted or updated data
- Restore dropped objects
- Create historical clones
- Perform point-in-time analysis

> **One-Line Definition:**  
> Time Travel allows users to access previous versions of data within the retention period.

---

# Why is Time Travel Needed?

In production, accidental changes can happen.

### Example

```sql
DELETE FROM employee;
```

Instead of losing the data permanently, Time Travel lets you access the table before the delete operation.

---

# How Time Travel Works

Snowflake does **not** immediately delete old data.

It keeps previous versions of the data for a specified **retention period**.

```
Current Data
     │
     ▼
Historical Versions
     │
(Time Travel Period)
```

Once the retention period expires, the data moves to **Fail-safe**.

---

# Retention Period

Default:

```sql
DATA_RETENTION_TIME_IN_DAYS = 1
```

Increase retention:

```sql
ALTER TABLE employee
SET DATA_RETENTION_TIME_IN_DAYS = 7;
```

Check retention:

```sql
SHOW TABLES LIKE 'EMPLOYEE';
```

---

# Time Travel Methods

| Method | Purpose |
|---------|---------|
| OFFSET | Access data relative to current time |
| TIMESTAMP | Access data at an exact time |
| BEFORE | Access data before a specific SQL statement |

---

# 1. OFFSET

Retrieve data from a previous point relative to the current time.

### Syntax

```sql
SELECT *
FROM employee
AT(OFFSET => -seconds);
```

### Example (1 Hour Ago)

```sql
SELECT *
FROM employee
AT(OFFSET => -3600);
```

### Common Values

| Time | Seconds |
|------|---------|
| 30 Minutes | 1800 |
| 1 Hour | 3600 |
| 1 Day | 86400 |

---

# 2. TIMESTAMP

Retrieve data from an exact timestamp.

### Syntax

```sql
SELECT *
FROM employee
AT(TIMESTAMP => '2026-07-29 10:00:00');
```

---

# 3. BEFORE

Retrieve data before a specific SQL statement.

### Syntax

```sql
SELECT *
FROM employee
BEFORE(STATEMENT => 'query_id');
```

---

# Find Query ID

```sql
SELECT *
FROM TABLE(
INFORMATION_SCHEMA.QUERY_HISTORY()
);
```

---

# Recover Dropped Objects

Restore a dropped table:

```sql
UNDROP TABLE employee;
```

Restore a schema:

```sql
UNDROP SCHEMA hr_schema;
```

Restore a database:

```sql
UNDROP DATABASE company_db;
```

---

# Historical Clone

Create a clone from a previous version.

```sql
CREATE TABLE employee_backup
CLONE employee
AT(OFFSET => -86400);
```

Or

```sql
CREATE TABLE employee_backup
CLONE employee
AT(TIMESTAMP => '2026-07-29 10:00:00');
```

---

# Time Travel vs Fail-safe

| Time Travel | Fail-safe |
|--------------|-----------|
| User accessible | Not user accessible |
| Query historical data | No querying |
| Recover dropped objects | No UNDROP |
| Retention: 0–90 days | Fixed 7 days |
| Used for recovery | Used for disaster recovery |

---

# Limitations

- Works only within the retention period.
- Longer retention increases storage usage.
- Not a replacement for long-term backups.

---

# Real Project Example

A developer accidentally executes:

```sql
DELETE FROM SALES_TRANSACTION;
```

Recovery Steps:

1. Find the Query ID.

```sql
SELECT *
FROM TABLE(INFORMATION_SCHEMA.QUERY_HISTORY());
```

2. View data before deletion.

```sql
SELECT *
FROM SALES_TRANSACTION
BEFORE(STATEMENT => 'query_id');
```

3. Create a recovery table.

```sql
CREATE TABLE SALES_TRANSACTION_BACKUP
CLONE SALES_TRANSACTION
BEFORE(STATEMENT => 'query_id');
```

---

# Interview Questions

### What is Time Travel?

A Snowflake feature that allows users to access and recover historical versions of data within the configured retention period.

### Difference between AT and BEFORE?

| AT | BEFORE |
|----|---------|
| Uses OFFSET or TIMESTAMP | Uses Query ID |
| Retrieves data at a point in time | Retrieves data before a statement |

### How do you recover a dropped table?

```sql
UNDROP TABLE employee;
```

### What happens after Time Travel expires?

The data moves to **Fail-safe** for **7 days**, where it is not user-accessible.

---

# Quick Revision

- ✅ Historical Data
- ✅ Point-in-Time Recovery
- ✅ OFFSET
- ✅ TIMESTAMP
- ✅ BEFORE
- ✅ Query ID
- ✅ UNDROP
- ✅ Historical Clone
- ✅ Retention Period
- ✅ Fail-safe

---

## ⭐ One-Line Summary

**Snowflake Time Travel allows users to access and recover historical versions of data within a configured retention period using OFFSET, TIMESTAMP, or BEFORE (STATEMENT).**
