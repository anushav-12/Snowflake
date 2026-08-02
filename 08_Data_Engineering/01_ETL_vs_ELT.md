# ETL vs ELT

## What is ETL?

ETL stands for:

- Extract
- Transform
- Load

In ETL, data is transformed before it is loaded into the data warehouse.

```
Source Systems
      │
      ▼
Extract Data
      │
      ▼
Transform Data
      │
      ▼
Load into Warehouse
```

Example:

- Extract customer data from SQL Server
- Clean duplicate records
- Standardize date formats
- Load into Snowflake

---

## What is ELT?

ELT stands for:

- Extract
- Load
- Transform

In ELT, raw data is first loaded into Snowflake and then transformed using SQL.

```
Source Systems
      │
      ▼
Extract Data
      │
      ▼
Load into Snowflake
      │
      ▼
Transform using SQL
```

Snowflake's powerful compute engine makes ELT the preferred approach.

---

## ETL vs ELT

| ETL | ELT |
|------|------|
| Transform before loading | Transform after loading |
| Uses external processing | Uses Snowflake compute |
| More common in traditional systems | Preferred in modern cloud platforms |
| Slower for large datasets | Faster and more scalable |

---

## Why Snowflake uses ELT

- Separate compute and storage
- Scalable virtual warehouses
- SQL-based transformations
- Faster processing for large datasets

---

## Interview Questions

### Why is ELT preferred in Snowflake?

Because Snowflake provides scalable compute resources that efficiently perform transformations after loading data.

### ETL vs ELT?

ETL transforms data before loading, whereas ELT loads raw data first and performs transformations inside Snowflake.

---

## Summary

- ETL = Transform first
- ELT = Load first
- Snowflake mainly uses ELT
