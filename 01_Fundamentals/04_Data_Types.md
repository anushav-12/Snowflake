# Snowflake Data Types

## Overview

A **data type** defines the type of data that a column can store. Choosing the correct data type improves data integrity, storage efficiency, and query performance.

---

## Categories of Data Types

Snowflake supports five major categories:

- Numeric
- String
- Date & Time
- Boolean
- Semi-Structured

---

## 1. Numeric Data Types

Used to store numbers.

Common Types:

- INT
- INTEGER
- NUMBER
- FLOAT

Example:

```sql
EMP_ID INT,
SALARY NUMBER(10,2)
```

---

## 2. String Data Types

Used to store text values.

Common Types:

- STRING
- VARCHAR
- TEXT

Example:

```sql
EMP_NAME STRING,
CITY VARCHAR
```

> **Note:** `STRING` and `VARCHAR` are synonyms in Snowflake.

---

## 3. Date & Time Data Types

Used to store dates and timestamps.

| Data Type | Example |
|------------|----------|
| DATE | 2026-07-20 |
| TIME | 10:30:15 |
| TIMESTAMP | 2026-07-20 10:30:15 |

Example:

```sql
JOIN_DATE DATE,
LOGIN_TIME TIMESTAMP
```

---

## 4. Boolean Data Type

Stores logical values.

Values:

- TRUE
- FALSE

Example:

```sql
IS_ACTIVE BOOLEAN
```

---

## 5. Semi-Structured Data

Snowflake can store semi-structured data using the **VARIANT** data type.

Supported Formats:

- JSON
- XML
- Avro
- Parquet

Example:

```sql
EMPLOYEE_DETAILS VARIANT
```

---

## PostgreSQL vs Snowflake

| PostgreSQL | Snowflake |
|------------|------------|
| INT | INT |
| VARCHAR | STRING / VARCHAR |
| DATE | DATE |
| BOOLEAN | BOOLEAN |
| JSONB | VARIANT |

---

## Best Practices

- Choose the correct data type for each column.
- Use numeric types for calculations.
- Store dates using DATE or TIMESTAMP instead of STRING.
- Use BOOLEAN for true/false values.
- Use VARIANT for semi-structured data.

---

## Interview Questions

### What is a data type?

A data type defines what type of data a column can store.

---

### Which data type stores text?

STRING or VARCHAR.

---

### Which data type stores JSON?

VARIANT.

---

### Is STRING different from VARCHAR?

No. Both are synonyms in Snowflake.

---

## Summary

- Data types define the type of data stored in a column.
- Snowflake supports Numeric, String, Date & Time, Boolean, and Semi-Structured data types.
- VARIANT is used to store semi-structured data such as JSON.
- Selecting the correct data type improves performance and data quality.
