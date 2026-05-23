Snowflake Notes — Day 1 Basics
What is Snowflake?

entity["company","Snowflake","cloud data platform company"] is a cloud-based data platform used for:

Data warehousing
Data engineering
Data analytics
Data sharing
ETL/ELT workloads

It stores and processes large amounts of data in the cloud.

1. Core Snowflake Architecture (Most Important)

Snowflake mainly works using 3 layers.

3 Layers of Snowflake
1. Database Storage Layer
2. Compute Layer (Warehouse)
3. Cloud Services Layer
1. Database Storage Layer
What it does
Stores all data permanently
Data is compressed and optimized automatically
Important Points
Snowflake manages storage automatically
User does not manage files manually
Supports structured and semi-structured data
Example

Tables and databases are stored here.

CREATE TABLE employees (
   id INT,
   name STRING
);
2. Compute Layer (Warehouse)
What it does
Executes SQL queries
Performs data processing
Handles ETL/ELT jobs
Important Points
Warehouses do NOT store data
They only process data
Compute and storage are separate in Snowflake
Example
CREATE WAREHOUSE my_wh
WITH WAREHOUSE_SIZE = 'XSMALL';

Use warehouse:

USE WAREHOUSE my_wh;
3. Cloud Services Layer
What it does

Handles:

Authentication
Security
Query optimization
Metadata management
Access control
Simple Understanding

This is the “brain” of Snowflake.

Architecture
Layer	Purpose
Storage Layer	Stores data
Compute Layer	Runs queries
Cloud Services	Manages everything
