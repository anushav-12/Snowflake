# Snowflake Caching

Snowflake has three types of cache.

```
            Query
               │
               ▼
        Result Cache
               │
        Warehouse Cache
               │
        Metadata Cache
```

---

## 1. Result Cache

Stores the final query result.

If the same query is executed again and data hasn't changed, Snowflake returns the cached result instantly.

Example

```sql
SELECT * FROM employee;
```

Running it again may return results without scanning the table.

---

## 2. Warehouse Cache

Stores data read by the Virtual Warehouse in memory.

Only available while the warehouse is running.

If the warehouse suspends, this cache is cleared.

---

## 3. Metadata Cache

Stores information about Micro-partitions.

Includes:

- Min values
- Max values
- Row count
- NULL count

Used for Partition Pruning.

---

## Cache Comparison

| Cache | Stores | Cleared When |
|--------|--------|--------------|
|Result Cache|Query Results|Data changes|
|Warehouse Cache|Table Data|Warehouse Suspends|
|Metadata Cache|Partition Metadata|Automatically Managed|

---

## Interview Questions

Difference between Result Cache and Warehouse Cache?

Which cache is cleared when Warehouse suspends?

Which cache helps Partition Pruning?

---

## Summary

Result Cache → Query results

Warehouse Cache → Memory

Metadata Cache → Partition information
