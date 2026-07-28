# Snowpipe

## What is Snowpipe?

Snowpipe is Snowflake's **serverless continuous data ingestion service** that automatically loads newly arrived files from a stage into a Snowflake table.

It continuously monitors a stage for new files and automatically executes the `COPY INTO` command to load them into the target table.

Snowpipe eliminates the need to manually run `COPY INTO` whenever new data arrives.

---

# Why Snowpipe?

Suppose an application generates a CSV file every few minutes.

Without Snowpipe:

```
Application
      │
      ▼
Upload File
      │
      ▼
Stage
      │
      ▼
Run COPY INTO manually
      │
      ▼
Table
```

Someone (or a scheduled job) must execute the COPY command every time.

With Snowpipe:

```
Application
      │
      ▼
Upload File
      │
      ▼
Stage
      │
      ▼
Snowpipe detects new file
      │
      ▼
Automatically executes COPY INTO
      │
      ▼
Table
```

No manual execution is required.

---

# Key Features

- Continuous data ingestion
- Automatic data loading
- Serverless
- Near real-time processing
- Uses COPY INTO internally
- Supports Internal and External Stages
- Supports Auto-Ingest

---

# Snowpipe Architecture

```
                Upload File
                     │
                     ▼
          Internal / External Stage
                     │
                     ▼
          Snowpipe monitors stage
                     │
                     ▼
       Executes COPY INTO automatically
                     │
                     ▼
            Snowflake Target Table
```

---

# Prerequisites

Before creating Snowpipe, you need:

- Target Table
- File Format
- Stage (Internal or External)

---

# Step 1 : Create File Format

```sql
CREATE FILE FORMAT csv_format
TYPE = CSV
FIELD_DELIMITER = ','
SKIP_HEADER = 1;
```

---

# Step 2 : Create Stage

```sql
CREATE STAGE sales_stage
FILE_FORMAT = csv_format;
```

---

# Step 3 : Create Target Table

```sql
CREATE TABLE sales
(
    id INT,
    customer_name STRING,
    amount NUMBER
);
```

---

# Step 4 : Create Pipe

```sql
CREATE PIPE sales_pipe
AS
COPY INTO sales
FROM @sales_stage;
```

Whenever a new file arrives in the stage, Snowpipe automatically loads it into the table.

---

# Internal Stage

Files are uploaded inside Snowflake.

```
Local File
      │
      ▼
Internal Stage
      │
      ▼
Snowpipe
      │
      ▼
Table
```

---

# External Stage

Files remain in cloud storage.

```
AWS S3
Azure Blob
Google Cloud Storage
        │
        ▼
External Stage
        │
        ▼
Snowpipe
        │
        ▼
Table
```

External Stages are commonly used in production.

---

# Auto-Ingest

Auto-Ingest automatically triggers Snowpipe whenever a new file is uploaded.

Example:

```
Application
      │
      ▼
AWS S3 Bucket
      │
      ▼
Cloud Notification
      │
      ▼
Snowpipe
      │
      ▼
COPY INTO
      │
      ▼
Snowflake Table
```

Supported Cloud Notification Services

| Cloud | Notification Service |
|--------|----------------------|
| AWS | SNS + SQS |
| Azure | Event Grid |
| Google Cloud | Pub/Sub |

---

# Snowpipe Workflow

```
Create Table
       │
Create File Format
       │
Create Stage
       │
Create Pipe
       │
Upload File
       │
Snowpipe detects file
       │
COPY INTO executes
       │
Data loaded into table
```

---

# Snowpipe vs COPY INTO

| Feature | COPY INTO | Snowpipe |
|----------|-----------|-----------|
| Loading | Manual | Automatic |
| Trigger | User/Scheduler | File Arrival |
| Best For | Batch Loading | Continuous Loading |
| Warehouse Required | Yes | No |
| Processing | On Demand | Near Real-Time |

---

# Important Commands

## Show Pipes

```sql
SHOW PIPES;
```

---

## Describe Pipe

```sql
DESC PIPE sales_pipe;
```

---

## Drop Pipe

```sql
DROP PIPE sales_pipe;
```

---

## Refresh Pipe

```sql
ALTER PIPE sales_pipe REFRESH;
```

Used when files were uploaded before the pipe was created or notifications were missed.

---

# Does Snowpipe Need a Warehouse?

No.

Snowpipe is **serverless**.

Snowflake automatically manages the compute resources required for loading data.

No Virtual Warehouse needs to be started.

---

# Does Snowpipe Use COPY INTO?

Yes.

Snowpipe internally executes the COPY INTO command.

Example:

```
CREATE PIPE sales_pipe
AS
COPY INTO sales
FROM @sales_stage;
```

Snowpipe simply automates this command.

---

# Duplicate File Handling

Snowflake maintains metadata of loaded files.

If the same file is uploaded again:

```
sales.csv
```

Snowpipe recognizes it has already been loaded and skips it.

To intentionally reload the same file:

```sql
COPY INTO sales
FROM @sales_stage
FORCE = TRUE;
```

---

# Advantages

- Fully automated loading
- Near real-time ingestion
- Serverless
- Highly scalable
- Minimal maintenance
- Integrates with cloud storage
- Reduces ETL effort

---

# Limitations

- Near real-time, not true real-time
- Not ideal for large historical backfills
- Auto-Ingest requires cloud event notifications
- Small compute cost for ingestion

---

# Real-World Use Cases

### Banking

Transaction files arrive every minute.

Snowpipe automatically loads them.

---

### E-Commerce

Orders generated continuously.

Snowpipe loads new order files automatically.

---

### IoT

Sensors continuously generate data files.

Snowpipe ingests them without manual intervention.

---

### Log Analytics

Application logs are uploaded continuously.

Snowpipe keeps analytics tables updated.

---

# Best Practices

- Use External Stages in production.
- Use Auto-Ingest whenever possible.
- Organize files into folders by date.
- Validate file formats before loading.
- Monitor pipe status regularly.
- Remove processed files if no longer required.
- Use proper error handling in COPY INTO.

---

# Interview Questions

## 1. What is Snowpipe?

Snowpipe is Snowflake's serverless continuous data ingestion service that automatically loads files from a stage into a table.

---

## 2. Does Snowpipe replace COPY INTO?

No.

Snowpipe internally executes COPY INTO automatically.

---

## 3. Is Snowpipe serverless?

Yes.

Snowflake manages the compute resources automatically.

---

## 4. Does Snowpipe require a Warehouse?

No.

It is serverless.

---

## 5. What triggers Snowpipe?

Arrival of a new file in the stage.

---

## 6. What is Auto-Ingest?

Auto-Ingest uses cloud event notifications to notify Snowpipe whenever a new file is uploaded.

---

## 7. Which stages are supported?

- Internal Stage
- External Stage

---

## 8. What is a Pipe?

A Pipe is a Snowflake object that stores the COPY INTO statement used by Snowpipe.

---

## 9. Is Snowpipe real-time?

No.

It is near real-time because it processes files in micro-batches.

---

## 10. How does Snowpipe avoid duplicate loading?

Snowflake keeps metadata of previously loaded files and skips files that have already been processed.

---

## 11. When would you use COPY INTO instead of Snowpipe?

COPY INTO is preferred for:
- One-time data loads
- Historical data loads
- Manual batch processing

Snowpipe is preferred for:
- Continuous ingestion
- Streaming pipelines
- Automated ETL

---

# Quick Revision

```
Source File
      │
      ▼
File Format
      │
      ▼
Stage
      │
      ▼
Snowpipe
      │
      ▼
COPY INTO
      │
      ▼
Snowflake Table
```

---

# Keywords

- Continuous Data Ingestion
- Serverless
- Near Real-Time
- COPY INTO
- Pipe
- Auto-Ingest
- Internal Stage
- External Stage
- Event Notifications
- Micro-Batches

---

# Summary

- Snowpipe automatically loads newly arrived files.
- It is built on top of COPY INTO.
- No warehouse is required.
- Supports Internal and External Stages.
- Auto-Ingest uses cloud event notifications.
- Best suited for continuous data ingestion.
- Provides near real-time loading using micro-batches.
