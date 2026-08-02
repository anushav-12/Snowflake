# Streams and Tasks

## What is a Stream?

A Stream tracks changes made to a table.

It records:

- INSERTS
- UPDATES
- DELETES

Streams enable incremental data processing.

---

## What is a Task?

A Task is a scheduler.

It automatically executes SQL statements at a specified time or interval.

Examples:

- Every minute
- Every hour
- Daily

---

## How Streams and Tasks work together

```
CSV File
    │
    ▼
Internal Stage
    │
    ▼
COPY INTO
    │
    ▼
Raw Table
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

Flow:

1. Data is loaded into the Raw Table.
2. Stream detects new or changed rows.
3. Task runs automatically.
4. Task processes only the changed records.
5. Target table is updated.

---

## Advantages

- Fully automated pipeline
- Incremental processing
- Lower compute cost
- Faster execution

---

## Interview Questions

### Difference between Stream and Task?

A Stream tracks data changes, while a Task schedules and executes SQL statements.

### Can a Stream run automatically?

No. A Stream only records changes. A Task or another process must consume those changes.

---

## Summary

- Stream = Detect changes
- Task = Execute automatically
