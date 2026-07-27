# Snowflake File Formats

## What is a File Format?

A File Format in Snowflake tells Snowflake **how to read a file** during loading or unloading.

It defines rules such as:
- File type
- Delimiter
- Header rows
- Null values
- Compression
- Date and Time formats

Without a File Format, Snowflake doesn't know how to interpret the file contents.

---

## Why Do We Need File Formats?

Suppose you have a CSV file:

ID,NAME,SALARY
1,Anusha,50000
2,Rahul,60000

Snowflake needs to know:
- Is this CSV?
- Does it have a header?
- What separates columns?
- How are NULL values represented?

A File Format answers these questions.

---

## Supported File Types

- CSV
- JSON
- PARQUET
- AVRO
- ORC
- XML

---

## Creating a File Format

```sql
CREATE FILE FORMAT my_csv_format
TYPE = CSV
FIELD_DELIMITER = ','
SKIP_HEADER = 1;
```

---

## Common Properties

### TYPE

Specifies file type.

Example

```sql
TYPE = CSV
```

---

### FIELD_DELIMITER

Specifies column separator.

Example

```sql
FIELD_DELIMITER = ','
```

---

### SKIP_HEADER

Skips header rows.

Example

```sql
SKIP_HEADER = 1
```

---

### FIELD_OPTIONALLY_ENCLOSED_BY

Handles quoted strings.

Example

```
"John","Bangalore"
```

```sql
FIELD_OPTIONALLY_ENCLOSED_BY='"'
```

---

### NULL_IF

Specifies values treated as NULL.

```sql
NULL_IF=('NULL','NA')
```

---

### COMPRESSION

Supported values

- AUTO
- GZIP
- BZ2
- BROTLI
- ZSTD

Example

```sql
COMPRESSION=AUTO
```

---

## Viewing File Formats

```sql
SHOW FILE FORMATS;
```

---

## Deleting File Format

```sql
DROP FILE FORMAT my_csv_format;
```

---

# Interview Questions

### What is a File Format?

A File Format defines how Snowflake reads or writes data files.

---

### Is File Format mandatory?

Yes, either explicitly or implicitly. If one isn't specified, Snowflake uses default settings.

---

### Which file types does Snowflake support?

CSV, JSON, Parquet, Avro, ORC and XML.

---

### Can one File Format be reused?

Yes. A File Format is reusable across multiple stages and COPY commands.
