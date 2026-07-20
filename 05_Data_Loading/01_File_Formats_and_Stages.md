# Snowflake Notes – File Formats & Stages

> A concise, interview-friendly guide to **Snowflake File Formats** and **Stages** for Data Engineering.

---

#  What is a File Format?

A **File Format** in Snowflake defines **how data inside a file should be interpreted** while loading or unloading data.

Instead of specifying parsing options every time you load a file, you create a reusable **named file format**.

**Think of it as:**

> *"Instructions that tell Snowflake how to read or write a file."*

Without a file format, Snowflake may not correctly understand delimiters, headers, compression, JSON structure, etc. File formats are applied during data loading (`COPY INTO <table>`) or unloading—not when simply uploading files to a stage. ([Snowflake Docs][1])

---

# Why Use File Formats?

 Reusable across multiple COPY commands

 Keeps SQL clean

 Prevents parsing errors

 Ensures consistent data loading

---

# Supported File Types

| Format  | Description             |
| ------- | ----------------------- |
| CSV     | Comma-separated files   |
| JSON    | Semi-structured JSON    |
| PARQUET | Columnar storage        |
| AVRO    | Row-based binary format |
| ORC     | Optimized Row Columnar  |
| XML     | XML documents           |

Snowflake supports both structured and semi-structured file formats for bulk data loading. ([Snowflake Docs][1])

---

# Common File Format Options

## CSV

```sql
CREATE FILE FORMAT csv_format
TYPE = CSV
FIELD_DELIMITER = ','
SKIP_HEADER = 1
NULL_IF = ('NULL')
EMPTY_FIELD_AS_NULL = TRUE;
```

### Important Options

| Option                       | Purpose                |
| ---------------------------- | ---------------------- |
| TYPE                         | File type              |
| FIELD_DELIMITER              | Column separator       |
| SKIP_HEADER                  | Skip header rows       |
| NULL_IF                      | Treat value as NULL    |
| COMPRESSION                  | GZIP, AUTO, etc.       |
| FIELD_OPTIONALLY_ENCLOSED_BY | Handles quoted strings |
| TRIM_SPACE                   | Removes extra spaces   |

---

## JSON

```sql
CREATE FILE FORMAT json_format
TYPE = JSON;
```

Useful for:

* APIs
* Event data
* Nested documents

---

## PARQUET

```sql
CREATE FILE FORMAT parquet_format
TYPE = PARQUET;
```

Ideal for:

* Data Lakes
* Spark
* Large analytics workloads

---

# Viewing File Formats

```sql
SHOW FILE FORMATS;
```

```sql
DESCRIBE FILE FORMAT csv_format;
```

---

# Dropping File Format

```sql
DROP FILE FORMAT csv_format;
```

---

# What is a Stage?

A **Stage** is a temporary storage location where files are placed **before loading into Snowflake tables** or **after unloading from tables**.

Think of a stage as a **bridge between storage and tables**.

```
CSV File
    │
    ▼
 Stage
    │
    ▼
COPY INTO
    │
    ▼
Snowflake Table
```

Stages simplify bulk data loading and unloading by storing or referencing data files. ([Snowflake Docs][2])

---

# Why Do We Need Stages?

Instead of loading files directly:

```
CSV
 ↓
Table
```

Snowflake follows:

```
CSV
 ↓
Stage
 ↓
COPY INTO
 ↓
Table
```

Benefits:

* Faster loading
* Reusable files
* Bulk processing
* Better security
* Error handling

---

# Types of Stages

## 1. User Stage

Every Snowflake user automatically gets a personal stage.

Syntax:

```sql
@~
```

Example:

```sql
PUT file://sales.csv @~;
```

Use case:

* Personal testing
* Temporary uploads

---

## 2. Table Stage

Automatically created for every table.

Syntax:

```sql
@%table_name
```

Example:

```sql
PUT file://orders.csv @%orders;
```

Use case:

* Files specific to one table

---

## 3. Named Internal Stage

Created manually.

```sql
CREATE STAGE my_stage;
```

Upload:

```sql
PUT file://sales.csv @my_stage;
```

Load:

```sql
COPY INTO sales
FROM @my_stage;
```

Best for:

* Shared projects
* Production pipelines
* Team access

---

## 4. External Stage

References files stored outside Snowflake.

Supports:

* Amazon S3
* Azure Blob Storage
* Google Cloud Storage

Example:

```sql
CREATE STAGE ext_stage
URL='s3://mybucket/data/'
FILE_FORMAT = csv_format;
```

No need to upload files into Snowflake.

---

# Stage Comparison

| Stage Type           | Storage      | Created By | Typical Use               |
| -------------------- | ------------ | ---------- | ------------------------- |
| User Stage           | Snowflake    | Automatic  | Personal uploads          |
| Table Stage          | Snowflake    | Automatic  | Table-specific files      |
| Named Internal Stage | Snowflake    | User       | Shared/team projects      |
| External Stage       | S3/Azure/GCS | User       | Cloud storage integration |

---

# Creating a Stage

```sql
CREATE STAGE sales_stage;
```

With File Format

```sql
CREATE STAGE sales_stage
FILE_FORMAT = csv_format;
```

---

# Upload File to Stage

```sql
PUT file://sales.csv @sales_stage;
```

---

# List Files

```sql
LIST @sales_stage;
```

---

# Load Data into Table

```sql
COPY INTO sales
FROM @sales_stage
FILE_FORMAT = (FORMAT_NAME = csv_format);
```

---

# Unload Data

```sql
COPY INTO @sales_stage
FROM sales;
```

---

# Remove Files

```sql
REMOVE @sales_stage;
```

---

# Stage Lifecycle

```
Local File
      │
      ▼
PUT Command
      │
      ▼
Stage
      │
      ▼
COPY INTO Table
      │
      ▼
Snowflake Table
```

For unloading:

```
Snowflake Table
      │
      ▼
COPY INTO Stage
      │
      ▼
Stage
      │
      ▼
GET Command
      │
      ▼
Local File
```

---

# Relationship Between File Formats & Stages

```
CSV File
      │
      ▼
File Format
(How to read)
      │
      ▼
Stage
(Where file is stored)
      │
      ▼
COPY INTO
      │
      ▼
Table
```

### Remember:

* **File Format = How to read the file**
* **Stage = Where the file is stored**
* **COPY INTO = Loads data into a table**

---

# Interview Questions

### 1. What is a File Format?

A reusable object that defines how Snowflake should parse or write files (CSV, JSON, Parquet, etc.).

---

### 2. What is a Stage?

A storage location used to hold or reference files before loading data into Snowflake or after unloading data.

---

### 3. Types of Stages?

* User Stage
* Table Stage
* Named Internal Stage
* External Stage

---

### 4. Difference Between Internal & External Stage

| Internal Stage                       | External Stage                            |
| ------------------------------------ | ----------------------------------------- |
| Stores files inside Snowflake        | References files in cloud storage         |
| Managed by Snowflake                 | Managed in S3/Azure/GCS                   |
| Requires `PUT` to upload local files | Reads directly from cloud storage         |
| Good for temporary/shared loads      | Best for enterprise data lake integration |

---

### 5. What does `COPY INTO` do?

Loads data from a stage into a table or exports table data into a stage.

---

### 6. Can a Stage have a default File Format?

**Yes.**

```sql
CREATE STAGE sales_stage
FILE_FORMAT = csv_format;
```

---

# Quick Revision

| Object      | Purpose                                  |
| ----------- | ---------------------------------------- |
| File Format | Defines how Snowflake reads/writes files |
| Stage       | Stores or references files               |
| PUT         | Uploads local files to an internal stage |
| GET         | Downloads files from an internal stage   |
| COPY INTO   | Loads or unloads data                    |
| LIST        | Displays staged files                    |
| REMOVE      | Deletes staged files                     |

---

# One-Line Summary

> **File Format tells Snowflake *how* to read a file, Stage tells Snowflake *where* the file is located, and `COPY INTO` moves data between stages and tables.** ([Snowflake Docs][2])

[1]: https://docs.snowflake.com/en/user-guide/data-load-prepare?utm_source=chatgpt.com "Preparing to load data | Snowflake Documentation"
[2]: https://docs.snowflake.com/en/sql-reference/ddl-stage?utm_source=chatgpt.com "Data loading / unloading DDL | Snowflake Documentation"
