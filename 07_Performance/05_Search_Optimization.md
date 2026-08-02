# Search Optimization Service

## What is Search Optimization?

Search Optimization is a Snowflake feature that speeds up highly selective lookups.

It is useful when queries search for a small number of rows in very large tables.

---

## Example

```sql
SELECT *
FROM customers
WHERE customer_id = 10001;
```

Instead of scanning many Micro-partitions, Search Optimization quickly locates matching rows.

---

## When should you use it?

- Point lookups
- Large tables
- Frequently searched columns

---

## Difference Between Clustering and Search Optimization

| Clustering | Search Optimization |
|------------|--------------------|
|Improves range queries|Improves point lookups|
|Uses Cluster Keys|Uses search access paths|
|Lower maintenance|Additional storage cost|

---

## Advantages

- Faster selective queries
- Less data scanned
- Better performance

---

## Limitations

- Additional cost
- Not required for every table

---

## Interview Questions

When would you use Search Optimization?

Difference between Search Optimization and Clustering?

---

## Summary

Search Optimization accelerates selective queries on very large tables.
