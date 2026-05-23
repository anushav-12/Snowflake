# Snowflake Notes — Day 1 Basics

## What is Snowflake?

Snowflake is a cloud-based data platform used for:

- Data warehousing
- Data engineering
- Data analytics
- Data sharing
- ETL/ELT workloads

It stores and processes large amounts of data in the cloud.

---

# Core Snowflake Architecture

Snowflake mainly works using 3 layers:

1. Database Storage Layer
2. Compute Layer (Warehouse)
3. Cloud Services Layer

---

## 1. Database Storage Layer

### What it does
- Stores all data permanently
- Data is compressed and optimized automatically

### Important Points
- Snowflake manages storage automatically
- User does not manage files manually
- Supports structured and semi-structured data

### Example

Tables and databases are stored here.

```sql
CREATE TABLE employees (
   id INT,
   name STRING
);
```

---

## 2. Compute Layer (Warehouse)

### What it does
- Executes SQL queries
- Performs data processing
- Handles ETL/ELT jobs

### Important Points
- Warehouses do NOT store data
- They only process data
- Compute and storage are separate in Snowflake

### Example

```sql
CREATE WAREHOUSE my_wh
WITH WAREHOUSE_SIZE = 'XSMALL';
```

Use warehouse:

```sql
USE WAREHOUSE my_wh;
```

---

## 3. Cloud Services Layer

### What it does

Handles:
- Authentication
- Security
- Query optimization
- Metadata management
- Access control

### Simple Understanding

This is the “brain” of Snowflake.

---

# Architecture Summary

| Layer | Purpose |
|---|---|
| Storage Layer | Stores data |
| Compute Layer | Runs queries |
| Cloud Services | Manages everything |
