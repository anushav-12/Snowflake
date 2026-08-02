# Micro-partitions

## What are Micro-partitions?

Micro-partitions are the fundamental storage units in Snowflake.

Whenever data is loaded into a table, Snowflake automatically divides it into small compressed storage units called **Micro-partitions**.

Each micro-partition stores approximately **50 MB to 500 MB** of **uncompressed** data.

Unlike traditional databases, users do **not** create or manage partitions manually.

Snowflake handles everything automatically.

---

## Why are Micro-partitions needed?

Without partitions, Snowflake would need to scan the entire table every time a query runs.

Micro-partitions allow Snowflake to read only the required data, making queries much faster.

Benefits:

- Faster query performance
- Less data scanned
- Lower compute cost
- Automatic partition management

---

## How do Micro-partitions work?

Suppose we have an Employee table.

| EmpID | Name | Department |
|-------|------|------------|
|1|Alice|HR|
|2|Bob|IT|
|3|Charlie|Finance|
|...|...|...|

After loading data:

Employee Table

```
Employee Table
      │
      ▼
┌─────────────┐
│ Micro Part 1│
└─────────────┘

┌─────────────┐
│ Micro Part 2│
└─────────────┘

┌─────────────┐
│ Micro Part 3│
└─────────────┘
```

Snowflake automatically creates these partitions.

---

## Metadata Stored

Each Micro-partition stores metadata.

Example:

```
Micro-partition

Minimum Salary = 25000

Maximum Salary = 98000

Rows = 15000

Distinct Departments

NULL Count
```

Snowflake uses this metadata instead of scanning every row.

---

## Partition Pruning

Partition pruning means skipping unnecessary partitions.

Example

Query:

```sql
SELECT *
FROM employee
WHERE salary > 90000;
```

Suppose

```
Partition 1

Salary 10000 - 25000

Partition 2

Salary 26000 - 50000

Partition 3

Salary 90000 - 120000
```

Snowflake scans only Partition 3.

```
Query
   │
   ▼
Metadata Check
   │
 ┌─┴──────────────┐
 │                │
Skip P1      Skip P2
 │                │
 └──────┬─────────┘
        ▼
     Scan P3
```

This is called **Partition Pruning**.

---

## Immutable Micro-partitions

Micro-partitions are immutable.

This means they cannot be modified after creation.

If you update or delete data,

Snowflake creates a new Micro-partition and marks the old one as obsolete.

Example

```
Old Partition
      │
UPDATE
      │
      ▼
New Partition Created
```

This design enables:

- Time Travel
- Fail-safe
- Zero-Copy Cloning

---

## Advantages

- Automatic partitioning
- Compression
- Fast queries
- No indexes required
- Better scalability
- Lower storage cost

---

## Limitations

- Users cannot manually manage partitions.
- Poorly ordered data may reduce pruning efficiency.

---

## Interview Questions

### Why doesn't Snowflake use indexes?

Because Snowflake uses metadata stored inside Micro-partitions for Partition Pruning.

---

### What does immutable mean?

Once a Micro-partition is created, it cannot be modified.

Updates create new partitions.

---

### What is Partition Pruning?

Skipping unnecessary Micro-partitions using metadata.

---

## Summary

- Automatic partitioning
- 50–500 MB (uncompressed)
- Stores metadata
- Supports Partition Pruning
- Immutable
- Improves query performance
