# Snowflake Stages

## What is a Stage?

A Stage is a location where files are stored before loading into Snowflake or after unloading from Snowflake.

Think of it as a temporary storage area.

---

## Why Do We Need Stages?

Without a Stage:

```
Local File → Table
```

With a Stage:

```
Local File
      ↓
    Stage
      ↓
Snowflake Table
```

Stages make loading faster, reusable and more manageable.

---

# Types of Stages

## 1. User Stage

Created automatically for every user.

Notation:

```
@
```

Example

```sql
LIST @;
```

Use Case

- Personal testing
- Temporary uploads

---

## 2. Table Stage

Automatically created for every table.

Notation

```
%table_name
```

Example

```sql
LIST @%CUSTOMERS;
```

Use Case

- Files specific to one table

---

## 3. Named Internal Stage

User-created stage inside Snowflake.

Example

```sql
CREATE STAGE my_stage;
```

Use Case

- Shared across multiple users
- Shared across multiple tables

---

## 4. External Stage

Points to cloud storage.

Supported Services

- AWS S3
- Azure Blob Storage
- Google Cloud Storage

Example

```sql
CREATE STAGE ext_stage
URL='s3://mybucket/files/';
```

---

## Listing Files

```sql
LIST @my_stage;
```

---

## Removing Files

```sql
REMOVE @my_stage;
```

---

## Dropping Stage

```sql
DROP STAGE my_stage;
```

---

# Stage Comparison

| Stage | Created By | Shared | Storage |
|--------|------------|---------|---------|
| User Stage | Automatic | No | Snowflake |
| Table Stage | Automatic | No | Snowflake |
| Named Stage | User | Yes | Snowflake |
| External Stage | User | Yes | Cloud Storage |

---

# Interview Questions

### What is a Stage?

A Stage is a storage location used to load or unload files.

---

### Which Stage is automatically created for every user?

User Stage.

---

### Which Stage is automatically created for every table?

Table Stage.

---

### Which Stage is commonly used in production?

Named Internal Stage or External Stage.

---

### Which cloud providers are supported?

AWS S3, Azure Blob Storage and Google Cloud Storage.
