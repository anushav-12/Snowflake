# Query Profile

## What is Query Profile?

Query Profile is a graphical execution plan showing how Snowflake executed a query.

It helps identify slow operations and optimize performance.

---

## Information Available

- Execution Time
- Bytes Scanned
- Partitions Scanned
- Join Operations
- Aggregations
- Sorting
- Filters

---

## Why use Query Profile?

To identify performance bottlenecks.

Example

```
Query

↓

Scan

↓

Filter

↓

Join

↓

Aggregation

↓

Result
```

Snowflake shows how much time each step took.

---

## Best Practices

- Reduce scanned partitions
- Filter early
- Avoid SELECT *
- Use proper Cluster Keys

---

## Interview Questions

What is Query Profile?

How do you optimize a slow query?

What information does Query Profile provide?

---

## Summary

Query Profile helps analyze and optimize query performance.
