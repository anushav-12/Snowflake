# 🌊 Snowflake Streams

## What is a Stream?

A **Stream** is a Snowflake object that **tracks changes (CDC - Change Data Capture)** made to a table.

It records:

- INSERT
- UPDATE
- DELETE

A stream **does not store data**. It only stores information about **what changed** since it was last consumed.

> **One-Line Definition:**  
> A Stream is a Change Data Capture (CDC) object that tracks inserts, updates, and deletes made to a table.

---

# Why Do We Need Streams?

In ETL pipelines, we usually need to process **only new or changed records**, not the entire table.

Without Stream:

```
Source Table
      │
      ▼
Read Entire Table Every Time
```

With Stream:

```
Source Table
      │
      ▼
Stream
(Only Changed Rows)
      │
      ▼
ETL Process
```

This makes data processing faster and more efficient.

---

# How Streams Work

A stream keeps track of changes made to a source table.

Example:

Initial Table

| ID | NAME |
|----|------|
| 1 | John |
| 2 | Alex |

Operations:

```sql
INSERT INTO employee VALUES (3,'David');

UPDATE employee
SET name='John Smith'
WHERE id=1;

DELETE FROM employee
WHERE id=2;
```

The stream records these changes.

---

# Create a Stream

### Syntax

```sql
CREATE STREAM employee_stream
ON TABLE employee;
```

---

# View Stream Data

```sql
SELECT *
FROM employee_stream;
```

The output contains only the rows that changed.

---

# Consume Stream Data

Example:

```sql
INSERT INTO employee_history
SELECT *
FROM employee_stream;
```

After successful consumption, the stream becomes empty.

---

# Stream Lifecycle

```
Table Changed
      │
      ▼
Stream Captures Changes
      │
      ▼
Task / SQL Reads Stream
      │
      ▼
Stream Becomes Empty
```

When new changes occur, the stream captures them again.

---

# METADATA Columns

A stream automatically adds metadata columns.

| Column | Purpose |
|---------|----------|
| METADATA$ACTION | INSERT or DELETE |
| METADATA$ISUPDATE | TRUE if part of an UPDATE |
| METADATA$ROW_ID | Unique identifier |

Example:

| ID | NAME | ACTION |
|----|------|---------|
| 1 | John | DELETE |
| 1 | John Smith | INSERT |

An UPDATE appears as:

- DELETE (old row)
- INSERT (new row)

---

# Types of Streams

### Standard Stream

Tracks:

- INSERT
- UPDATE
- DELETE

---

### Append-Only Stream

Tracks only:

- INSERT

Ignores UPDATE and DELETE.

Create:

```sql
CREATE STREAM employee_stream
ON TABLE employee
APPEND_ONLY = TRUE;
```

---

# Stream + Task

Streams are commonly used with Tasks.

```
Source Table
      │
      ▼
Stream
      │
      ▼
Task
      │
      ▼
Target Table
```

The Task automatically processes new records whenever changes exist.

---

# Advantages

- Tracks only changed data
- Supports Change Data Capture (CDC)
- Faster ETL processing
- Reduces unnecessary data movement
- Works well with Tasks

---

# Limitations

- Does not store actual data permanently
- Must be consumed before retention expires
- Cannot recover old versions like Time Travel
- Used only for tracking changes

---

# Streams vs Time Travel

| Stream | Time Travel |
|---------|-------------|
| Tracks data changes | Accesses historical data |
| Used for CDC | Used for recovery |
| Supports ETL | Supports querying old versions |
| Consumed after reading | Historical data remains until retention ends |

---

# Streams vs Tasks

| Stream | Task |
|---------|------|
| Detects changes | Executes SQL |
| Stores change information | Automates processing |
| CDC object | Scheduler |

---

# Real Project Example

### Scenario

A sales table receives new records every hour.

Instead of processing the full table:

1. Create a Stream.

```sql
CREATE STREAM sales_stream
ON TABLE sales;
```

2. Read only changed rows.

```sql
SELECT *
FROM sales_stream;
```

3. A Task loads the changed rows into the warehouse.

---

# Interview Questions

### What is a Stream?

A Stream is a Snowflake CDC object that tracks INSERT, UPDATE, and DELETE operations on a table.

---

### Does a Stream store data?

**No.**

It stores **change information**, not the actual table data.

---

### What happens after reading a Stream?

After the changes are successfully consumed, the stream becomes empty and waits for new changes.

---

### How are UPDATE operations stored?

As two records:

- DELETE (old row)
- INSERT (new row)

---

### Why are Streams used?

To process **only changed records**, making ETL pipelines faster and more efficient.

---

# Quick Revision

- ✅ Tracks INSERT, UPDATE, DELETE
- ✅ CDC Object
- ✅ Reads Only Changed Data
- ✅ Used with Tasks
- ✅ Supports Incremental Loading
- ✅ UPDATE = DELETE + INSERT
- ❌ Not a Backup
- ❌ Does Not Store Table Data

---

## ⭐ One-Line Summary

**A Stream is a Change Data Capture (CDC) object that tracks changes made to a table, enabling incremental data processing in ETL pipelines.**
