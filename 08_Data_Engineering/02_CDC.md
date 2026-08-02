# Change Data Capture (CDC)

## What is CDC?

Change Data Capture (CDC) is the process of identifying changes made to data.

These changes include:

- INSERT
- UPDATE
- DELETE

Instead of processing an entire table every time, only the changed records are processed.

---

## Why use CDC?

Without CDC:

```
1 Million Rows

↓

Process all 1 Million rows
```

With CDC:

```
1 Million Rows

↓

Only 150 changed rows processed
```

This improves performance and reduces compute costs.

---

## CDC in Snowflake

Snowflake implements CDC using **Streams**.

A Stream tracks changes made to a table.

It stores information about:

- New rows
- Updated rows
- Deleted rows

The actual data remains in the source table.

---

## Example

Customer Table

```
ID   Name

1    Alice

2    Bob
```

Update:

```sql
UPDATE customer
SET name='Robert'
WHERE id=2;
```

The Stream records the update, allowing downstream processes to handle only the changed row.

---

## Advantages

- Incremental processing
- Faster ETL pipelines
- Lower compute cost
- Better scalability

---

## Interview Questions

### What is CDC?

A technique for identifying only changed records instead of processing an entire table.

### How is CDC implemented in Snowflake?

Using Streams.

---

## Summary

- Detects Inserts
- Detects Updates
- Detects Deletes
- Uses Streams
