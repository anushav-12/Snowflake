# Snowflake Architecture

## Overview

Snowflake is a cloud-native data platform that separates **Storage**, **Compute**, and **Cloud Services**. This architecture allows each layer to scale independently, providing better performance, concurrency, and cost efficiency.

---

## Architecture

```text
                Users
                  │
                  ▼
         Cloud Services Layer
                  │
                  ▼
     Virtual Warehouse (Compute)
                  │
                  ▼
           Storage Layer
```

---

## 1. Storage Layer

The Storage Layer stores all persistent data.

**Stores:**
- Databases
- Schemas
- Tables
- Views
- Internal Metadata
- Semi-structured data (JSON, Parquet, Avro)

**Key Points**
- Stores data permanently
- Automatically compressed
- Automatically encrypted
- Does **not** execute queries

---

## 2. Compute Layer (Virtual Warehouse)

A Virtual Warehouse is an independent compute cluster used to execute SQL queries.

**Responsibilities**
- Execute SQL queries
- Load data
- Perform ETL/ELT
- Join, Sort and Aggregate data

**Key Points**
- Compute can be started or stopped anytime.
- Multiple warehouses can access the same data.
- Compute and Storage are independent.

---

## 3. Cloud Services Layer

The Cloud Services Layer manages the entire Snowflake platform.

**Responsibilities**
- Authentication
- Authorization
- Metadata Management
- Query Optimization
- Transaction Management
- Security

---

## Query Execution Flow

```text
User
 │
 ▼
Cloud Services
 │
 ▼
Virtual Warehouse
 │
 ▼
Storage Layer
 │
 ▼
Virtual Warehouse
 │
 ▼
User
```

---

## PostgreSQL vs Snowflake

| PostgreSQL | Snowflake |
|------------|------------|
| Storage and Compute are together | Storage and Compute are separate |
| Single server | Multiple Virtual Warehouses |
| Limited concurrency | High concurrency |
| Manual scaling | Independent scaling |

---

## Advantages

- Independent Storage & Compute
- Better Performance
- High Concurrency
- Auto Scaling
- Cost Effective
- No Infrastructure Management

---

## Important Questions

### What are the three layers of Snowflake?

- Storage Layer
- Compute Layer (Virtual Warehouse)
- Cloud Services Layer

---

### Which layer executes SQL queries?

**Virtual Warehouse (Compute Layer)**

---

### Which layer stores data?

**Storage Layer**

---

### What is the biggest advantage of Snowflake Architecture?

Storage and Compute are independent, allowing each to scale separately.

---

## Summary

- Storage stores data.
- Virtual Warehouse processes data.
- Cloud Services manages the platform.
- Storage and Compute are completely independent.
