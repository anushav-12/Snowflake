# Cost Optimization

## Why is Cost Optimization Important?

Snowflake charges separately for:

- Compute
- Storage

Reducing unnecessary compute usage lowers costs.

---

## Best Practices

### 1. Enable Auto Suspend

Stops idle warehouses automatically.

---

### 2. Enable Auto Resume

Starts warehouses only when required.

---

### 3. Select Required Columns

Avoid:

```sql
SELECT *
FROM employee;
```

Prefer:

```sql
SELECT emp_id,
emp_name
FROM employee;
```

---

### 4. Filter Early

Apply WHERE clauses to reduce scanned data.

---

### 5. Use Partition Pruning

Store data efficiently to reduce scanned partitions.

---

### 6. Use Appropriate Warehouse Size

Avoid oversized warehouses for small workloads.

---

### 7. Use Result Cache

Repeated queries may return cached results without additional compute.

---

### 8. Use MERGE for Incremental Loading

Process only new or changed records instead of reloading entire tables.

---

## Interview Questions

### How do you reduce Snowflake costs?

- Auto Suspend
- Auto Resume
- Partition Pruning
- Result Cache
- Proper Warehouse Size
- Incremental Loading

---

## Summary

Efficient warehouse usage and optimized SQL reduce Snowflake costs.
