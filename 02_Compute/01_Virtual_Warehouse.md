# Virtual Warehouse

## Overview

A **Virtual Warehouse** is Snowflake's compute engine responsible for executing SQL queries and processing data. It does **not** store data; all data is stored in the **Storage Layer**.

---

## Why do we need a Virtual Warehouse?

A Virtual Warehouse provides the compute resources required to:

- Execute SQL queries
- Load data
- Perform ETL/ELT operations
- Join tables
- Sort and aggregate data
- Run stored procedures

Without a Virtual Warehouse, SQL queries cannot be executed.

---

## Snowflake Architecture

```text
           User
             │
             ▼
     Cloud Services
             │
             ▼
   Virtual Warehouse
      (Compute)
             │
             ▼
      Storage Layer
```

---

## Compute Resources

Warehouse provides compute resources such as CPU and RAM.

- CPU performs calculations, joins, sorting and aggregations.
- RAM temporarily holds data while queries are being processed.

These resources process data stored in the Storage Layer.

> **Note:** Increasing the warehouse size increases compute resources, **not storage**.
Increasing the warehouse size does **not** move or duplicate the data.
Only the compute resources (CPU and RAM) increase, allowing queries to execute faster.
---

## Responsibilities

A Virtual Warehouse is responsible for:

- Executing SQL queries
- Loading data
- Performing transformations
- Sorting data
- Joining tables
- Aggregating data

It **does not** store data.

---

## Multiple Virtual Warehouses

Multiple Virtual Warehouses can access the same Storage Layer simultaneously.

```text
                Storage
                   │
      ┌────────────┼────────────┐
      ▼            ▼            ▼
 Warehouse A  Warehouse B  Warehouse C
```

Each warehouse has its own CPU and RAM, allowing different workloads to run independently.

---

## Compute Isolation

Using separate Virtual Warehouses prevents one workload from affecting another.

Example:

- Warehouse A → ETL Jobs
- Warehouse B → BI Reports
- Warehouse C → Dashboard Queries

This improves performance and concurrency.

---

## Warehouse Sizes

- X-Small
- Small
- Medium
- Large
- X-Large

Larger warehouses provide more CPU and memory, allowing queries to execute faster.

---

## Auto Suspend

Automatically stops an idle warehouse after a specified period to reduce compute costs.

---

## Auto Resume

Automatically starts a suspended warehouse when a new query is executed.

---

## PostgreSQL vs Snowflake

| PostgreSQL | Snowflake |
|------------|------------|
| CPU & RAM are part of the database server | CPU & RAM are provided by a Virtual Warehouse |
| Storage and Compute are together | Storage and Compute are separated |
| Scaling usually requires upgrading the server | Compute can be scaled independently |

---

## Best Practices

- Use **X-Small** warehouses for development and learning.
- Enable **AUTO_SUSPEND** to reduce credit usage.
- Enable **AUTO_RESUME** for convenience.
- Use separate warehouses for ETL, BI, and Reporting workloads.

---

## Important Questions

### What is a Virtual Warehouse?

A Virtual Warehouse is Snowflake's compute engine that executes SQL queries and processes data.

---

### Does a Virtual Warehouse store data?

No. Data is stored in the Storage Layer.

---

### Why are multiple Virtual Warehouses used?

To isolate workloads, improve concurrency, and allow different teams to work independently.

---

### What are compute resources?

Compute resources refer to the **CPU** and **RAM** used by a Virtual Warehouse to process queries.

---

### What happens when you increase the warehouse size?

The Virtual Warehouse receives more compute resources (CPU and RAM), allowing queries to execute faster. The data remains in the same Storage Layer.

---

## Summary

- A Virtual Warehouse is Snowflake's compute engine.
- It executes queries but does not store data.
- Data is stored only in the Storage Layer.
- Multiple warehouses can access the same data simultaneously.
- Larger warehouses provide more CPU and RAM.
- Auto Suspend and Auto Resume help optimize compute costs.
