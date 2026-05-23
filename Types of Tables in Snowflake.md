# Types of Tables in Snowflake

Snowflake mainly supports 3 table types:

1. Permanent Table
2. Temporary Table
3. Transient Table

---

# 1. Permanent Table

## Simple Definition

Permanent tables are default tables used for storing important long-term data.

---

# Features

- Stored permanently
- Supports Time Travel
- Supports Fail-safe
- Best recovery protection

---

# Example

```sql
CREATE TABLE employees (
   id INT,
   name STRING
);
```

---

# Used For

- Production data
- Business-critical data
- Important records

---

# Important Point

Permanent tables provide maximum data protection.

---

# 2. Temporary Table

## Simple Definition

Temporary tables exist only during the current session.

After session ends:
- Table is automatically deleted

---

# Features

- Session-based
- Automatically removed after session ends
- Only visible to current user/session

---

# Example

```sql
CREATE TEMP TABLE temp_sales (
   id INT,
   amount NUMBER
);
```

---

# Used For

- Testing
- Temporary calculations
- Intermediate query processing

---

# Important Point

Temporary tables are not meant for permanent storage.

---

# 3. Transient Table

## Simple Definition

Transient tables are used for temporary or staging data that does not require full recovery protection.

---

# Features

- Lower storage cost
- No Fail-safe
- Supports limited Time Travel

---

# Example

```sql
CREATE TRANSIENT TABLE stage_data (
   id INT,
   value STRING
);
```

---

# Used For

- ETL staging tables
- Temporary business data
- Re-creatable datasets

---

# Important Point

Transient tables reduce storage cost because Fail-safe is not maintained.

---

# Permanent vs Temporary vs Transient

| Feature | Permanent | Temporary | Transient |
|---|---|---|---|
| Permanent Storage | Yes | No | Yes |
| Session Based | No | Yes | No |
| Time Travel | Yes | Limited | Limited |
| Fail-safe | Yes | No | No |
| Cost | Higher | Lower | Lower |

---

# Time Travel and UNDROP

## DROP TABLE

```sql
DROP TABLE employees;
```

Deletes the table.

---

## UNDROP TABLE

```sql
UNDROP TABLE employees;
```

Restores dropped table using Time Travel.

---

# Important Point

UNDROP works mainly for:
- Permanent tables
- Transient tables (limited)

Temporary tables usually cannot be recovered after session ends.

---

# Retention Period

| Table Type | Recovery Support |
|---|---|
| Permanent | Yes |
| Transient | Limited |
| Temporary | Usually No |

---

# Real-Time Understanding

| Table Type | Best Use Case |
|---|---|
| Permanent | Important business data |
| Temporary | Session/testing data |
| Transient | Staging and ETL data |

---

# Important Interview Points

- Permanent tables support Fail-safe
- Transient tables reduce storage cost
- Temporary tables exist only during session
- UNDROP works because of Time Travel

---

# Quick Revision Notes

```text
Permanent = Important long-term data
```

```text
Temporary = Session-only table
```

```text
Transient = Staging/temp business data
```

```text
Permanent → Full recovery
Transient → Limited recovery
Temporary → Usually no recovery
```

---

# What I Learned

- Snowflake has 3 main table types
- Permanent tables provide maximum protection
- Temporary tables are session-based
- Transient tables are useful for ETL/staging
- Time Travel helps recover dropped objects
