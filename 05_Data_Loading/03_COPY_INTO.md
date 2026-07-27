# COPY INTO Command

## What is COPY INTO?

COPY INTO is the primary command used to load data from a Stage into a Snowflake table.

It can also unload data from a table into files.

---

## Data Loading Flow

```
Local File
      ↓
    Stage
      ↓
COPY INTO
      ↓
Snowflake Table
```

---

## Basic Syntax

```sql
COPY INTO customers
FROM @my_stage
FILE_FORMAT = (FORMAT_NAME = my_csv_format);
```

---

## Loading Specific Files

```sql
COPY INTO customers
FROM @my_stage
FILES=('customers.csv');
```

---

## Loading Multiple Files

```sql
COPY INTO customers
FROM @my_stage
FILES=('a.csv','b.csv');
```

---

## Pattern Matching

```sql
COPY INTO customers
FROM @my_stage
PATTERN='.*csv';
```

---

# ON_ERROR Options

## ABORT_STATEMENT

Stops loading immediately if any error occurs.

Default option.

```sql
ON_ERROR='ABORT_STATEMENT'
```

---

## CONTINUE

Skips bad records and continues loading.

```sql
ON_ERROR='CONTINUE'
```

---

## SKIP_FILE

Skips the entire file if an error is found.

```sql
ON_ERROR='SKIP_FILE'
```

---

## FORCE

Reloads files even if they were loaded previously.

```sql
FORCE=TRUE
```

---

# Validation

Validate files without loading.

```sql
COPY INTO customers
FROM @my_stage
VALIDATION_MODE='RETURN_ERRORS';
```

---

# Copy History

View previous load history.

```sql
SELECT *
FROM TABLE(INFORMATION_SCHEMA.COPY_HISTORY());
```

---

# Complete Example

```sql
CREATE FILE FORMAT csv_format
TYPE=CSV
SKIP_HEADER=1;

CREATE STAGE customer_stage;

COPY INTO customers
FROM @customer_stage
FILE_FORMAT=(FORMAT_NAME=csv_format)
ON_ERROR='CONTINUE';
```

---

# Best Practices

- Use Named Stages in production.
- Reuse File Formats.
- Validate files before loading.
- Monitor COPY_HISTORY.
- Use FORCE only when necessary.
- Use CONTINUE carefully to avoid unnoticed bad records.

---

# Interview Questions

### What is COPY INTO?

It is the command used to load data from a Stage into a Snowflake table or unload data from a table into files.

---

### What is the default ON_ERROR option?

ABORT_STATEMENT.

---

### What does FORCE=TRUE do?

Reloads files even if they were previously loaded.

---

### How can you load only one file?

```sql
FILES=('customers.csv')
```

---

### How can you load only CSV files?

```sql
PATTERN='.*csv'
```

---

### How do you check load history?

```sql
SELECT *
FROM TABLE(INFORMATION_SCHEMA.COPY_HISTORY());
```
