# Warehouse Sizing

## What is Warehouse Sizing?

A Virtual Warehouse can be resized depending on workload.

Larger warehouses provide more compute power but consume more credits.

---

## Warehouse Sizes

```
X-Small

↓

Small

↓

Medium

↓

Large

↓

X-Large

↓

2X-Large

↓

3X-Large

↓

4X-Large
```

Each increase approximately doubles the compute resources.

---

## When to use each size?

### X-Small

- Learning
- Development
- Small reports

---

### Small

- Daily ETL
- Small production jobs

---

### Medium

- Large joins
- Moderate data processing

---

### Large and Above

- Massive ETL pipelines
- Large analytical workloads
- Heavy concurrent users

---

## Auto Suspend

Automatically suspends the warehouse after inactivity.

Example:

```sql
AUTO_SUSPEND = 60
```

Warehouse suspends after 60 seconds.

---

## Auto Resume

Automatically starts the warehouse when a query is submitted.

---

## Best Practices

- Use the smallest warehouse that meets performance needs.
- Enable Auto Suspend.
- Enable Auto Resume.
- Resize only when necessary.

---

## Interview Questions

### Can warehouse size be changed?

Yes.

Warehouses can be resized at any time.

### Does resizing affect stored data?

No.

Storage is separate from compute.

---

## Summary

- More size = More compute
- More compute = More credits
- Use Auto Suspend and Auto Resume
