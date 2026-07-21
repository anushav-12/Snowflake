# Snowflake-06-File-Formats-and-Stages.md

# Snowflake Notes – File Formats & Stages

> **Goal:** Understand how Snowflake loads data from files into tables and the role of **File Formats**, **Stages**, and **COPY INTO**.

---

# Data Loading Architecture

Before learning File Formats and Stages, understand the complete workflow.

```
Local File
     │
     ▼
File Format
(How to read the file)
     │
     ▼
Stage
(Where the file is located)
     │
     ▼
COPY INTO
(Loads the data)
     │
     ▼
Snowflake Table
```

Every data loading process in Snowflake follows this flow.

---

# What is a File Format?

A **File Format** is a reusable Snowflake object that tells Snowflake **how to read or write a file**.

It contains **instructions**, not data.

Think of it as:

> **"A set of rules that tells Snowflake how to interpret a file."**

For example, Snowflake needs to know:

* Is the file CSV or JSON?
* What separates the columns?
* Does the file contain a header?
* Which values should be treated as NULL?
* What encoding is used?

Without these instructions, Snowflake cannot correctly parse the file.

---

# Why Do We Need File Formats?

Suppose we have this CSV file:

```csv
EMP_ID,NAME,SALARY
1,John,50000
2,Alice,60000
```

How does Snowflake know:

* It's a CSV?
* Values are separated by commas?
* The first row is a header?
* Empty values should become NULL?
* The file uses UTF-8 encoding?

It doesn't.

That's why we create a **File Format**.

---

# File Formats Do NOT Store Data

A common interview question:

**Does a File Format store data?**

**Answer: No.**

A File Format only stores **instructions** about how Snowflake should read or write files.

---

# Supported File Types

| File Type | Common Use                     |
| --------- | ------------------------------ |
| CSV       | Tabular data, Excel exports    |
| JSON      | APIs, nested data              |
| PARQUET   | Big Data, Data Lakes, Spark    |
| AVRO      | Streaming pipelines, Kafka     |
| ORC       | Hadoop/Hive analytics          |
| XML       | Legacy enterprise applications |

---

# Creating a CSV File Format

```sql
CREATE FILE FORMAT employee_csv
TYPE = CSV
FIELD_DELIMITER = ','
SKIP_HEADER = 1
NULL_IF = ('NULL')
EMPTY_FIELD_AS_NULL = TRUE;
```

---

# Important File Format Options

| Option              | Purpose                            |
| ------------------- | ---------------------------------- |
| TYPE                | Specifies the file type            |
| FIELD_DELIMITER     | Character separating columns       |
| SKIP_HEADER         | Skips header rows                  |
| NULL_IF             | Converts specific values into NULL |
| EMPTY_FIELD_AS_NULL | Treats empty fields as NULL        |
| COMPRESSION         | Specifies compression type         |
| ENCODING            | Character encoding (UTF-8, etc.)   |

---

# Quick Examples

### CSV

```sql
CREATE FILE FORMAT csv_format
TYPE = CSV;
```

---

### JSON

```sql
CREATE FILE FORMAT json_format
TYPE = JSON;
```

Used for:

* APIs
* Nested objects
* Event data

---

### PARQUET

```sql
CREATE FILE FORMAT parquet_format
TYPE = PARQUET;
```

Best for:

* Data Engineering
* Spark
* Analytics

---

# Managing File Formats

View all file formats:

```sql
SHOW FILE FORMATS;
```

Describe a file format:

```sql
DESCRIBE FILE FORMAT employee_csv;
```

Delete a file format:

```sql
DROP FILE FORMAT employee_csv;
```

---

# What is a Stage?

A **Stage** is a location where Snowflake **stores (Internal Stage)** or **references (External Stage)** files before loading them into tables or after unloading data.

Think of a Stage as a **folder** that Snowflake can access.

```
employees.csv
      │
      ▼
 Stage
      │
      ▼
COPY INTO
      │
      ▼
Employees Table
```

---

# Why Do We Need a Stage?

Snowflake runs in the cloud.

It **cannot directly access files stored on your laptop**.

Example:

```
C:\Users\Anusha\Downloads\employees.csv
```

Snowflake has no access to your local computer.

Therefore:

1. Upload the file to a Stage.
2. Snowflake reads the file from the Stage.
3. COPY INTO loads the data into the table.

---

# Think of a Stage Like This

Imagine receiving a package.

```
Courier
    │
    ▼
Front Door (Stage)
    │
    ▼
Living Room (Table)
```

The package isn't placed directly in the living room.

It first arrives at the front door.

Similarly, files first reach a Stage before being loaded into a table.

---

# Types of Stages

Snowflake has **two categories** of stages.

```
Stages
│
├── Internal Stage
│
└── External Stage
```

---

# Internal Stage

Files are stored **inside Snowflake**.

Snowflake manages the storage.

Best for:

* Learning
* Testing
* Small projects
* Internal workflows

Internal stages are further divided into three types.

---

## 1. User Stage

Every Snowflake user automatically gets a personal stage.

Reference:

```sql
@~
```

Example:

```sql
PUT file://employees.csv @~;
```

Best for:

* Personal testing
* Temporary uploads

Think:

> **My personal folder**

---

## 2. Table Stage

Every table automatically gets its own stage.

Reference:

```sql
@%employees
```

Example:

```sql
PUT file://employees.csv @%employees;
```

Best for:

* Files belonging to one table

Think:

> **This folder belongs to one table**

---

## 3. Named Internal Stage

Created manually by the user.

```sql
CREATE STAGE employee_stage;
```

Reference:

```sql
@employee_stage
```

Best for:

* Shared projects
* Multiple files
* Reusable data loading

Think:

> **A shared folder created by us**

---

# External Stage

An External Stage **does not store files inside Snowflake**.

Instead, it references files stored in cloud storage such as:

* Amazon S3
* Azure Blob Storage
* Google Cloud Storage

```
AWS S3
   │
   ▼
External Stage
   │
   ▼
COPY INTO
   │
   ▼
Snowflake Table
```

External Stages are commonly used in production environments.

---

# Internal vs External Stage

| Internal Stage                 | External Stage                    |
| ------------------------------ | --------------------------------- |
| Files stored inside Snowflake  | Files remain in cloud storage     |
| Managed by Snowflake           | Managed in S3/Azure/GCS           |
| Uses PUT to upload local files | Reads directly from cloud storage |
| Best for learning and testing  | Best for production pipelines     |

---

# Creating a Named Stage

```sql
CREATE STAGE employee_stage;
```

Create a stage with a default file format:

```sql
CREATE STAGE employee_stage
FILE_FORMAT = employee_csv;
```

---

# Upload a File (PUT)

Upload a local file into an Internal Stage.

```sql
PUT file://C:/Users/Anusha/Downloads/employees.csv
@employee_stage;
```

**PUT works only with Internal Stages.**

---

# View Files in a Stage

```sql
LIST @employee_stage;
```

Example output:

```
employees.csv
```

---

# Load Data into a Table

```sql
COPY INTO employees
FROM @employee_stage
FILE_FORMAT = (FORMAT_NAME = employee_csv);
```

Snowflake:

* Finds the file in the Stage
* Uses the File Format
* Loads the data into the table

---

# Complete Data Loading Flow

```
employees.csv (Local Machine)
             │
             ▼
      CREATE FILE FORMAT
             │
             ▼
        CREATE STAGE
             │
             ▼
             PUT
             │
             ▼
      @employee_stage
             │
             ▼
    LIST @employee_stage
             │
             ▼
         COPY INTO
             │
             ▼
      Employees Table
             │
             ▼
SELECT * FROM employees;
```

---

# Understanding the Workflow

Every object has one responsibility.

| Object      | Responsibility                           |
| ----------- | ---------------------------------------- |
| File        | Contains the actual data                 |
| File Format | Defines how to read the file             |
| Stage       | Defines where the file is located        |
| PUT         | Uploads local files to an Internal Stage |
| LIST        | Displays staged files                    |
| COPY INTO   | Loads data into a table                  |
| Table       | Stores the final data                    |

---

# Real-World Production Flow

During learning:

```
Laptop
   │
   ▼
Internal Stage
   │
   ▼
COPY INTO
   │
   ▼
Snowflake Table
```

In real companies:

```
Application
      │
      ▼
Amazon S3
      │
      ▼
External Stage
      │
      ▼
COPY INTO
      │
      ▼
Snowflake Table
```

Applications usually place files directly into cloud storage.

Snowflake simply reads those files.

---

# File Format vs Stage

| File Format                            | Stage                                 |
| -------------------------------------- | ------------------------------------- |
| Defines **how** Snowflake reads a file | Defines **where** the file is located |
| Stores instructions                    | Stores or references files            |
| Required during loading                | Required before loading               |

---

# Interview Questions

## 1. What is a File Format?

A reusable object that tells Snowflake how to read or write files.

---

## 2. Does a File Format store data?

No.

It stores only instructions.

---

## 3. What is a Stage?

A location where Snowflake stores or references files before loading or after unloading data.

---

## 4. Why can't Snowflake load files directly from your laptop?

Because Snowflake runs in the cloud and cannot directly access files stored on a user's local machine.

The file must first be uploaded to an Internal Stage or stored in cloud storage and accessed through an External Stage.

---

## 5. Name the different Stage types.

* User Stage
* Table Stage
* Named Internal Stage
* External Stage

---

## 6. Difference between Internal and External Stage?

| Internal                      | External                      |
| ----------------------------- | ----------------------------- |
| Files stored inside Snowflake | Files stored in cloud storage |
| Uses PUT                      | No PUT required               |
| Good for learning             | Used in production            |

---

## 7. What does PUT do?

Uploads a local file into an Internal Stage.

---

## 8. What does LIST do?

Displays the files available inside a Stage.

---

## 9. What does COPY INTO do?

Loads data from a Stage into a Snowflake table.

---

## 10. Which Stage is most commonly used while learning?

Named Internal Stage.

---

## 11. Which Stage is commonly used in production?

External Stage connected to Amazon S3, Azure Blob Storage, or Google Cloud Storage.

---

# Quick Revision

| Object      | Remember This        |
| ----------- | -------------------- |
| File        | Actual data          |
| File Format | How to read the file |
| Stage       | Where the file is    |
| PUT         | Upload file          |
| LIST        | Check uploaded files |
| COPY INTO   | Load data            |
| Table       | Final destination    |

---

# Final Summary

```
                SNOWFLAKE DATA LOADING

           employees.csv (Local File)
                     │
                     ▼
              File Format
      (How should I read this file?)
                     │
                     ▼
                  Stage
        (Where is the file located?)
                     │
                     ▼
                 COPY INTO
          (Load the data into table)
                     │
                     ▼
             Snowflake Table
```

## Remember Forever

Snowflake needs to know **three things** before loading data:

1. **What is the data?** → File
2. **How should I read it?** → File Format
3. **Where is it located?** → Stage

Finally,

**`COPY INTO` moves the data from the Stage into the Snowflake table.**
