# Snowflake Tables - Complete Notes

## Overview

A table is the primary object used to store data in Snowflake.

Unlike traditional databases such as PostgreSQL, Oracle, or SQL Server, Snowflake automatically manages storage using micro-partitions and does not require manual partition management or traditional indexes.

---

# Types of Tables in Snowflake

Snowflake supports three main table types:

1. Permanent Table
2. Temporary Table
3. Transient Table

---

# 1. Permanent Table

## Definition

Permanent tables are the default table type used for storing important business and production data.

## Example

```sql
CREATE TABLE employees (
    id NUMBER,
    name STRING
);
```

## Features

* Stored permanently
* Supports Time Travel
* Supports Fail-safe
* Maximum data protection
* Default table type

## Use Cases

* Production data
* Customer records
* Financial data
* Business-critical information

---

# 2. Temporary Table

## Definition

Temporary tables exist only for the duration of the current session.

Once the session ends, Snowflake automatically removes the table.

## Example

```sql
CREATE TEMP TABLE temp_sales (
    id NUMBER,
    amount NUMBER
);
```

## Features

* Session-specific
* Automatically deleted after session ends
* Visible only within the current session
* No long-term storage

## Use Cases

* Testing
* Temporary calculations
* Intermediate query results
* Session-based processing

## Important Point

Temporary tables cannot typically be recovered once the session ends.

---

# 3. Transient Table

## Definition

Transient tables are designed for temporary or staging data that does not require full recovery protection.

## Example

```sql
CREATE TRANSIENT TABLE stage_data (
    id NUMBER,
    value STRING
);
```

## Features

* Permanent storage until dropped
* Supports Time Travel
* Does NOT support Fail-safe
* Lower storage cost

## Use Cases

* ETL staging tables
* Temporary business data
* Re-creatable datasets
* Data pipeline processing

## Important Point

Transient tables are cheaper because Snowflake does not maintain Fail-safe storage.

---

# Permanent vs Temporary vs Transient

| Feature           | Permanent       | Temporary    | Transient   |
| ----------------- | --------------- | ------------ | ----------- |
| Permanent Storage | Yes             | No           | Yes         |
| Session Based     | No              | Yes          | No          |
| Time Travel       | Yes             | Limited      | Yes         |
| Fail-safe         | Yes             | No           | No          |
| Cost              | Higher          | Lower        | Lower       |
| Best For          | Production Data | Session Data | ETL/Staging |

---

# Micro-Partitions

## Definition

Snowflake stores table data internally in small storage units called micro-partitions.

Users do not create or manage micro-partitions.

Snowflake automatically creates and maintains them.

## Characteristics

* Automatically managed
* Columnar storage
* Immutable
* Typically 50–500 MB compressed

## Example

```text
EMPLOYEE TABLE

Micro-Partition 1
Micro-Partition 2
Micro-Partition 3
Micro-Partition 4
...
```

---

# Micro-Partition Metadata

For every micro-partition, Snowflake stores metadata.

## Metadata Examples

* Minimum value
* Maximum value
* Distinct values
* Null count

Example:

```text
Micro-Partition 1

Salary Min = 10000
Salary Max = 20000
```

```text
Micro-Partition 2

Salary Min = 20001
Salary Max = 30000
```

This metadata is used for query optimization.

---

# Partition Pruning

## Definition

Partition Pruning is the process where Snowflake eliminates unnecessary micro-partitions from a query scan.

Instead of scanning the entire table, Snowflake scans only relevant micro-partitions.

---

## Example

Micro-Partition Metadata:

```text
MP1 : Salary 10000 - 20000
MP2 : Salary 20001 - 30000
MP3 : Salary 30001 - 40000
MP4 : Salary 40001 - 50000
```

Query:

```sql
SELECT *
FROM employees
WHERE salary = 45000;
```

Snowflake Execution:

```text
MP1 -> Skip
MP2 -> Skip
MP3 -> Skip
MP4 -> Read
```

Only MP4 is scanned.

---

## Benefits

* Faster query execution
* Reduced I/O
* Lower compute cost
* Better performance

---

## Interview Definition

Partition Pruning is a query optimization technique where Snowflake uses micro-partition metadata to skip irrelevant partitions and scan only the partitions that may contain matching data.

---

# Time Travel

## Definition

Time Travel allows access to historical versions of data for a specified retention period.

It enables users to query, recover, and restore previous versions of data.

---

## Uses

* Recover deleted rows
* Recover updated data
* Restore dropped tables
* Query historical data

---

## Query Historical Data

View data from 5 minutes ago:

```sql
SELECT *
FROM employees
AT (OFFSET => -60*5);
```

---

## Query Using Timestamp

```sql
SELECT *
FROM employees
AT (TIMESTAMP => '2026-06-01 10:00:00');
```

---

## Restore Using Time Travel

```sql
DROP TABLE employees;
```

Recover:

```sql
UNDROP TABLE employees;
```

---

## Retention Period

| Edition    | Retention     |
| ---------- | ------------- |
| Standard   | 1 Day         |
| Enterprise | Up to 90 Days |

Example:

```sql
ALTER TABLE employees
SET DATA_RETENTION_TIME_IN_DAYS = 7;
```

---

# Fail-safe

## Definition

Fail-safe is Snowflake's final recovery mechanism after the Time Travel period expires.

Lifecycle:

```text
Active Data
      ↓
Time Travel
      ↓
Fail-safe (7 Days)
      ↓
Permanent Deletion
```

---

## Important Facts

* Available only for Permanent Tables
* Fixed duration of 7 days
* Users cannot access it directly
* Requires Snowflake Support for recovery

---

## Why Transient Tables Cost Less

Transient tables do not maintain Fail-safe storage.

Therefore:

```text
No Fail-safe
      ↓
Less Storage
      ↓
Lower Cost
```

---

# Time Travel vs Fail-safe

| Feature               | Time Travel   | Fail-safe         |
| --------------------- | ------------- | ----------------- |
| User Accessible       | Yes           | No                |
| Query Historical Data | Yes           | No                |
| Restore Data          | Yes           | Support Only      |
| Duration              | Up to 90 Days | Fixed 7 Days      |
| Purpose               | User Recovery | Disaster Recovery |

---

# DROP TABLE

## Definition

Removes a table from active use.

```sql
DROP TABLE employees;
```

The table becomes inaccessible but is not immediately deleted because Time Travel still protects it.

---

# UNDROP TABLE

## Definition

Restores a dropped table during the Time Travel retention period.

```sql
UNDROP TABLE employees;
```

Recovered Items:

* Table structure
* Data
* Metadata

---

## Example

```sql
DROP TABLE employees;

UNDROP TABLE employees;
```

The table returns exactly as it existed before the drop operation.

---

# DELETE vs TRUNCATE vs DROP

| Command  | Data Removed  | Structure Removed |
| -------- | ------------- | ----------------- |
| DELETE   | Selected Rows | No                |
| TRUNCATE | All Rows      | No                |
| DROP     | Entire Table  | Yes               |

---

## DELETE

```sql
DELETE FROM employees
WHERE id = 1;
```

Removes specific rows.

---

## TRUNCATE

```sql
TRUNCATE TABLE employees;
```

Removes all rows but keeps table structure.

---

## DROP

```sql
DROP TABLE employees;
```

Removes both table structure and data.

Can be recovered using:

```sql
UNDROP TABLE employees;
```

during the Time Travel retention period.

---

# Zero-Copy Cloning

## Definition

Zero-Copy Cloning creates an instant copy of a table without physically duplicating data.

## Example

```sql
CREATE TABLE emp_clone
CLONE employees;
```

## Benefits

* Instant creation
* No initial storage duplication
* Fast environment setup
* Efficient testing and development

## Common Use Cases

* Development environments
* Testing
* Backup copies
* Data validation

---

# Important 

## Does Snowflake Use Indexes?

Answer:

No. Snowflake primarily uses micro-partition metadata and partition pruning instead of traditional indexes.

---

## What is Partition Pruning?

Answer:

Partition Pruning is the process where Snowflake uses micro-partition metadata to eliminate irrelevant partitions and scan only the partitions that may contain matching data.

---

## Why are Transient Tables Cheaper?

Answer:

Transient tables do not maintain Fail-safe storage, which reduces storage costs.

---

## Difference Between Time Travel and Fail-safe?

Answer:

Time Travel allows users to query and recover historical data, while Fail-safe is an emergency recovery mechanism available only through Snowflake Support after Time Travel expires.

---

# Quick Notes

```text
Permanent Table
→ Time Travel + Fail-safe
→ Best for Production Data
```

```text
Temporary Table
→ Session Only
→ Auto Deleted After Session Ends
```

```text
Transient Table
→ Time Travel Only
→ No Fail-safe
→ Best for ETL/Staging
```

```text
Micro-Partitions
→ Automatic Storage Units
→ Managed by Snowflake
```

```text
Metadata
→ Min Value
→ Max Value
→ Distinct Values
→ Null Count
```

```text
Partition Pruning
→ Skip Unnecessary Partitions
→ Faster Queries
```

```text
Time Travel
→ Query Historical Data
→ Recover Dropped Objects
```

```text
Fail-safe
→ 7 Days
→ Support Team Recovery
```

```text
UNDROP
→ Restore Dropped Tables
```

```text
Zero-Copy Clone
→ Instant Copy
→ No Initial Data Duplication
```
