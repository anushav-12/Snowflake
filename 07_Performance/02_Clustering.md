# Clustering

## What is Clustering?

Clustering organizes related rows close together inside Micro-partitions.

This improves Partition Pruning.

---

## Why is Clustering needed?

Suppose your table contains 500 million sales records.

You frequently run:

```sql
SELECT *
FROM sales
WHERE order_date='2026-07-01';
```

Without clustering, Snowflake scans many Micro-partitions.

With clustering, only a few partitions are scanned.

---

## Cluster Keys

Example

```sql
CREATE TABLE sales(
id INT,
order_date DATE,
customer_id INT
)
CLUSTER BY(order_date);
```

Now Snowflake tries to keep similar dates together.

---

## When should you use Clustering?

Large tables

Frequently filtered columns

Repeated queries

Examples

- Date
- Customer ID
- Region

---

## Advantages

- Better Partition Pruning
- Faster queries
- Less compute usage

---

## Disadvantages

- Additional maintenance cost
- Not needed for small tables

---

## Best Practices

✔ Use only on very large tables

✔ Choose frequently filtered columns

✔ Avoid unnecessary clustering

---

## Interview Questions

When should you create Cluster Keys?

Difference between Partitioning and Clustering?

Does Snowflake automatically cluster data?

---

## Summary

Cluster Keys improve query performance by organizing related data together.
