# ⚙️ Snowflake Tasks

## What is a Task?

A **Task** is a Snowflake object used to **automate SQL execution**. It can run SQL statements on a schedule or trigger when data changes.

Tasks are commonly used to automate ETL/ELT pipelines.

> **One-Line Definition:**  
> A Task is a scheduler that automatically executes SQL statements at a specified time or when new data is available.

---

# Why Do We Need Tasks?

Instead of manually running SQL queries every time, Tasks automate the process.

Example:

- Load new records
- Transform data
- Update summary tables
- Refresh reports
- Process Stream data

---

# How Tasks Work

```
Schedule / Data Change
          │
          ▼
        Task
          │
          ▼
     Executes SQL
          │
          ▼
   Target Table Updated
```

---

# Create a Scheduled Task

### Syntax

```sql
CREATE TASK employee_task
WAREHOUSE = compute_wh
SCHEDULE = 'USING CRON 0 * * * * UTC'
AS
INSERT INTO employee_history
SELECT * FROM employee_stream;
```

The above task runs **every hour**.

---

# Start a Task

```sql
ALTER TASK employee_task RESUME;
```

---

# Pause a Task

```sql
ALTER TASK employee_task SUSPEND;
```

---

# Execute a Task Manually

```sql
EXECUTE TASK employee_task;
```

---

# Drop a Task

```sql
DROP TASK employee_task;
```

---

# Stream + Task

Tasks are commonly used with Streams for automated incremental loading.

```
Source Table
      │
      ▼
Stream
(Tracks Changes)
      │
      ▼
Task
(Runs SQL Automatically)
      │
      ▼
Target Table
```

Example:

```sql
INSERT INTO employee_history
SELECT *
FROM employee_stream;
```

The Task runs this automatically whenever scheduled.

---

# Triggered Task

Instead of running on a fixed schedule, a Task can run **only when the Stream has data**.

Example:

```sql
CREATE TASK employee_task
WAREHOUSE = compute_wh
WHEN SYSTEM$STREAM_HAS_DATA('employee_stream')
AS
INSERT INTO employee_history
SELECT *
FROM employee_stream;
```

This avoids unnecessary executions.

---

# Task Hierarchy

Tasks can depend on other Tasks.

```
Task A
   │
   ▼
Task B
   │
   ▼
Task C
```

Useful for multi-step ETL pipelines.

---

# Advantages

- Automates SQL execution
- Eliminates manual jobs
- Supports ETL/ELT pipelines
- Works seamlessly with Streams
- Supports task dependencies

---

# Limitations

- Executes only SQL statements
- Requires a warehouse to run
- Failed tasks need monitoring
- Poor scheduling can increase compute costs

---

# Tasks vs Streams

| Task | Stream |
|------|---------|
| Executes SQL | Tracks data changes |
| Automation | Change Data Capture (CDC) |
| Scheduler | Change tracker |
| Processes data | Detects changes |

---

# Tasks vs Snowpipe

| Task | Snowpipe |
|------|-----------|
| Automates SQL execution | Automatically loads new files |
| Used after data is in Snowflake | Used to ingest files into Snowflake |
| Works with Streams | Works with Stages |

---

# Real Project Example

### Scenario

A company receives sales data every hour.

Workflow:

1. Snowpipe loads files into the `sales` table.
2. Stream captures new records.
3. Task moves only changed records into the warehouse.

```
Stage
   │
   ▼
Snowpipe
   │
   ▼
Sales Table
   │
   ▼
Stream
   │
   ▼
Task
   │
   ▼
Warehouse Table
```

---

# Interview Questions

### What is a Task?

A Task is a Snowflake object that automates SQL execution on a schedule or when data changes.

---

### Why are Tasks used?

To automate ETL/ELT processes and eliminate manual execution.

---

### Can a Task run automatically?

**Yes.**

- On a schedule (CRON)
- When a Stream contains data

---

### Can a Task execute Python code?

**No.**

Tasks execute SQL statements (or invoke supported procedures), not arbitrary Python scripts directly.

---

### Why are Streams and Tasks used together?

- **Stream** detects new or changed data.
- **Task** automatically processes those changes.

---

# Quick Revision

- ✅ Automates SQL
- ✅ Scheduler
- ✅ Uses Warehouse
- ✅ CRON Scheduling
- ✅ Triggered by Streams
- ✅ Supports Task Chaining
- ✅ Used in ETL Pipelines

---

## ⭐ One-Line Summary

**A Task automates SQL execution on a schedule or when a Stream detects new data, enabling fully automated ETL pipelines.**
