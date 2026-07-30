Snowflake Time Travel
1. Introduction

Time Travel is a Snowflake feature that allows users to access historical versions of data.

It provides the ability to:

Query data from the past
Recover accidentally deleted data
Restore dropped objects
Create clones from historical data
Perform point-in-time analysis

Think of Time Travel as an undo mechanism for your data.

2. Why Time Travel is Required?

In real-world applications, accidental changes happen frequently.

Examples:

Scenario 1: Accidental DELETE

Before:

EMP_ID	NAME	SALARY
101	John	50000
102	Alex	60000

Developer runs:

DELETE FROM employee;

All data disappears.

Using Time Travel:

SELECT *
FROM employee
AT(OFFSET => -3600);

We can view the previous version.

Scenario 2: Wrong UPDATE

Before:

ID	SALARY
1	50000

Query executed:

UPDATE employee
SET salary = 100000;

Incorrect update.

Time Travel allows us to recover the previous state.

3. How Time Travel Works?

Snowflake does not immediately delete old data.

Instead, Snowflake maintains historical versions.

Example:

10:00 AM
Employee table created
        |
        |
11:00 AM
100 rows inserted
        |
        |
12:00 PM
Salary updated
        |
        |
1:00 PM
Current table

Snowflake stores previous versions internally.

During the retention period, users can access old versions.

4. Time Travel Retention Period

Time Travel works based on a retention period.

Default:

DATA_RETENTION_TIME_IN_DAYS = 1

Meaning:

Snowflake keeps historical data for 1 day.

Example:

Today
 |
 |
Previous 24 hours available
Change Retention Period

Example:

Keep data for 7 days:

ALTER TABLE employee
SET DATA_RETENTION_TIME_IN_DAYS = 7;

Check retention:

SHOW TABLES LIKE 'EMPLOYEE';

Look for:

retention_time
5. Time Travel Syntax

Snowflake provides three methods:

OFFSET
TIMESTAMP
STATEMENT ID
6. Using OFFSET

OFFSET retrieves data relative to current time.

Syntax:

SELECT *
FROM table_name
AT(
    OFFSET => -seconds
);
Example: 1 Hour Ago

1 hour:

60 minutes × 60 seconds

= 3600 seconds

Query:

SELECT *
FROM employee
AT(
    OFFSET => -3600
);

Meaning:

Current Time
     |
     |
     ↓
1 hour back
Example: 30 Minutes Ago

30 minutes:

30 × 60 = 1800 seconds

Query:

SELECT *
FROM employee
AT(
    OFFSET => -1800
);
Example: 2 Days Ago

Calculation:

2 × 24 × 60 × 60

=172800 seconds

Query:

SELECT *
FROM employee
AT(
    OFFSET => -172800
);
7. Using TIMESTAMP

Instead of calculating seconds, we can provide an exact time.

Syntax:

SELECT *
FROM table_name
AT(
TIMESTAMP => 'YYYY-MM-DD HH:MI:SS'
);

Example:

SELECT *
FROM employee
AT(
TIMESTAMP => '2026-07-29 10:00:00'
);

Returns:

The table state at that exact timestamp.

8. Using BEFORE Statement

Used when you know the query ID that caused the change.

Syntax:

SELECT *
FROM table_name
BEFORE(
STATEMENT => 'query_id'
);

Example:

SELECT *
FROM employee
BEFORE(
STATEMENT => '01b7c9a2-0000'
);

Returns:

Data before that SQL statement executed.

9. Finding Query ID

Use Query History:

SELECT *
FROM TABLE(
INFORMATION_SCHEMA.QUERY_HISTORY()
);

Find:

QUERY_ID

Use that ID in BEFORE.

10. Recover Dropped Objects

Time Travel supports UNDROP.

Restore Table

Dropped:

DROP TABLE employee;

Recover:

UNDROP TABLE employee;
Restore Schema
UNDROP SCHEMA hr_schema;
Restore Database
UNDROP DATABASE company_db;
11. Creating Historical Clone

Time Travel can create a clone from the past.

Example:

Create backup from yesterday:

CREATE TABLE employee_backup
CLONE employee
AT(
OFFSET => -86400
);

86400 seconds:

24 hours × 60 × 60
12. Time Travel with Clone

Example:

Current table:

EMPLOYEE

Yesterday's version:

EMPLOYEE_BACKUP

Command:

CREATE TABLE employee_backup
CLONE employee
AT(
TIMESTAMP=>'2026-07-29 10:00:00'
);
13. Objects Supporting Time Travel
Object	Supported
Permanent Tables	✅
Schemas	✅
Databases	✅
Views	Limited
Temporary Tables	Limited
Transient Tables	Limited
14. Time Travel Limitations
1. Retention Period

After retention expires:

Time Travel
      |
      ↓
Fail-safe
      |
      ↓
Permanent deletion
2. Storage Cost

More retention means:

More historical data
More storage consumption
3. Not a Long-Term Backup

Time Travel is for:

Short-term recovery
Accidental changes

Not:

Disaster recovery
Long-term archival
15. Time Travel vs Fail-safe
Feature	Time Travel	Fail-safe
User controlled	Yes	No
Duration	0-90 days	7 days
Purpose	Recovery & querying	Disaster recovery
SQL access	Yes	No
UNDROP	Yes	No
16. Time Travel vs Traditional Backup
Feature	Time Travel	Backup
Automatic	✅	Depends
Fast recovery	✅	Depends
Historical query	✅	Usually No
Extra backup files	❌	✅
Retention based	✅	Depends
17. Important Commands Cheat Sheet
Set Retention
ALTER TABLE employee
SET DATA_RETENTION_TIME_IN_DAYS = 7;
Query Previous Version
SELECT *
FROM employee
AT(
OFFSET => -3600
);
Query Specific Time
SELECT *
FROM employee
AT(
TIMESTAMP=>'2026-07-29 10:00:00'
);
Query Before Statement
SELECT *
FROM employee
BEFORE(
STATEMENT=>'query_id'
);
Restore Table
UNDROP TABLE employee;
Restore Schema
UNDROP SCHEMA schema_name;
Restore Database
UNDROP DATABASE database_name;
18. Interview Questions
Q1. What is Time Travel in Snowflake?

Answer:

Time Travel is a Snowflake feature that allows users to access historical versions of data and recover deleted or modified objects within the configured retention period.

Q2. How do you recover a dropped table?

Answer:

UNDROP TABLE table_name;
Q3. Difference between AT and BEFORE?
AT	BEFORE
Access data at specific time	Access data before a statement
Uses timestamp/offset	Uses query ID
Q4. Does Time Travel replace backup?

Answer:

No. Time Travel provides short-term recovery. It does not replace long-term backup strategies.

Q5. What happens after Time Travel expires?

Answer:

Data moves to Fail-safe. During Fail-safe, Snowflake support can recover data only for disaster recovery purposes.

19. Real Project Example

Scenario:

A production table:

SALES_TRANSACTION

Developer accidentally executes:

DELETE FROM SALES_TRANSACTION;

Solution:

Step 1:
Find query ID.

Step 2:

SELECT *
FROM SALES_TRANSACTION
BEFORE(
STATEMENT=>'query_id'
);

Step 3:

Create recovery table:

CREATE TABLE SALES_TRANSACTION_BACKUP
CLONE SALES_TRANSACTION
BEFORE(
STATEMENT=>'query_id'
);
Key Interview Keywords
✔ Historical Data
✔ Point-in-Time Recovery
✔ Retention Period
✔ OFFSET
✔ TIMESTAMP
✔ BEFORE
✔ Query ID
✔ UNDROP
✔ Fail-safe
✔ Zero Copy Clone
✔ Data Recovery
✔ Historical Query
One-line Summary

Snowflake Time Travel allows users to access and recover historical versions of data within a configured retention period using OFFSET, TIMESTAMP, or STATEMENT ID.
