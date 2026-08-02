# Resource Monitors

## What are Resource Monitors?

A Resource Monitor is a Snowflake feature that helps control and monitor warehouse credit usage.

It allows administrators to:

- Track credit consumption
- Set usage limits
- Receive notifications
- Automatically suspend warehouses

---

## Why are Resource Monitors needed?

Without monitoring, warehouses can continue running and consume unnecessary credits.

Resource Monitors help prevent unexpected costs.

---

## How do they work?

```
Warehouse
     │
     ▼
Consumes Credits
     │
     ▼
Resource Monitor
     │
     ├── Notify at 50%
     ├── Notify at 75%
     ├── Notify at 90%
     └── Suspend at 100%
```

---

## Example

```sql
CREATE RESOURCE MONITOR monthly_monitor
WITH CREDIT_QUOTA = 100
FREQUENCY = MONTHLY;

ALTER WAREHOUSE my_wh
SET RESOURCE_MONITOR = monthly_monitor;
```

---

## Benefits

- Prevents overspending
- Automatic warehouse suspension
- Better cost control
- Alerts administrators

---

## Interview Questions

### What is a Resource Monitor?

A Resource Monitor tracks and controls Snowflake credit consumption.

### Does it monitor storage?

No.

It only monitors compute credits used by Virtual Warehouses.

---

## Summary

- Monitors warehouse credits
- Sends alerts
- Suspends warehouses
- Reduces cloud costs
