# Snowflake Security - Role Based Access Control (RBAC)

## What is RBAC?

RBAC (Role-Based Access Control) is Snowflake's security model where:

- Privileges are granted to Roles.
- Roles are assigned to Users.
- Users inherit the privileges of their assigned roles.

Instead of granting permissions directly to users, permissions are managed through roles, making administration easier and more secure.

---

## RBAC Hierarchy

```
ACCOUNTADMIN
      │
SECURITYADMIN
      │
 SYSADMIN
      │
Custom Roles
      │
     Users
```

---

## Why RBAC?

Suppose a company has 100 Data Engineers.

❌ Without RBAC:
- Grant permissions individually to all 100 users.

✅ With RBAC:
- Create one role.
- Grant permissions to the role.
- Assign the role to all users.

If permissions change, update the role once instead of every user.

---

# Built-in Roles

## 1. ACCOUNTADMIN

Highest privilege role.

Responsibilities:
- Manage the entire Snowflake account
- Billing
- Warehouses
- Databases
- Security
- Users
- Roles

Best Practice:
- Avoid using ACCOUNTADMIN for daily work.

---

## 2. SECURITYADMIN

Responsible for security administration.

Can:
- Create Roles
- Grant/Revoke Roles
- Grant/Revoke Privileges
- Manage Security Policies

Example

```sql
CREATE ROLE DATA_ENGINEER;

GRANT ROLE DATA_ENGINEER TO USER ANUSHA;
```

---

## 3. USERADMIN

Responsible for user management.

Can:
- Create Users
- Alter Users
- Delete Users
- Reset Passwords

Example

```sql
CREATE USER ANUSHA
PASSWORD='Password123';
```

---

## 4. SYSADMIN

Responsible for creating and managing objects.

Can create:
- Databases
- Schemas
- Tables
- Views
- Stages
- Warehouses

Example

```sql
CREATE DATABASE SALES_DB;

CREATE SCHEMA SALES_DB.RAW;
```

---

## 5. PUBLIC

Every user automatically receives this role.

Best Practice:
- Avoid granting sensitive permissions to PUBLIC.

---

# Users

Create User

```sql
CREATE USER ANUSHA
PASSWORD='Password123'
DEFAULT_ROLE=DATA_ENGINEER
DEFAULT_WAREHOUSE=COMPUTE_WH;
```

Useful Properties

- DEFAULT_ROLE
- DEFAULT_WAREHOUSE
- DEFAULT_NAMESPACE
- DEFAULT_SECONDARY_ROLES

---

# Roles

Create Role

```sql
CREATE ROLE DATA_ENGINEER;
```

Grant Role to User

```sql
GRANT ROLE DATA_ENGINEER
TO USER ANUSHA;
```

Activate Role

```sql
USE ROLE DATA_ENGINEER;
```

---

# Privileges

Privileges define what a role can do.

Common Privileges

- USAGE
- SELECT
- INSERT
- UPDATE
- DELETE
- CREATE
- MODIFY
- MONITOR
- OWNERSHIP

Example

```sql
GRANT SELECT
ON TABLE SALES_DB.RAW.CUSTOMERS
TO ROLE DATA_ENGINEER;
```

---

# Why is USAGE Required?

To access a table, a role needs:

1. USAGE on Database
2. USAGE on Schema
3. Appropriate privilege on the object (SELECT, INSERT, etc.)

Example

```sql
GRANT USAGE
ON DATABASE SALES_DB
TO ROLE DATA_ENGINEER;

GRANT USAGE
ON SCHEMA SALES_DB.RAW
TO ROLE DATA_ENGINEER;

GRANT SELECT
ON TABLE SALES_DB.RAW.CUSTOMERS
TO ROLE DATA_ENGINEER;
```

Without USAGE, object-level privileges alone are not enough.

---

# Ownership

Every Snowflake object has one owner.

The owner can:

- Drop the object
- Rename the object
- Grant privileges
- Transfer ownership

Transfer Ownership

```sql
GRANT OWNERSHIP
ON TABLE SALES_DB.RAW.CUSTOMERS
TO ROLE DATA_ENGINEER;
```

Note:
Ownership is exclusive. An object can have only one owner at a time.

---

# Viewing Permissions

Show grants on a table

```sql
SHOW GRANTS ON TABLE SALES_DB.RAW.CUSTOMERS;
```

Show grants to a role

```sql
SHOW GRANTS TO ROLE DATA_ENGINEER;
```

Show roles

```sql
SHOW ROLES;
```

Show users

```sql
SHOW USERS;
```

---

# Switching Context

Switch Role

```sql
USE ROLE DATA_ENGINEER;
```

Switch Warehouse

```sql
USE WAREHOUSE COMPUTE_WH;
```

Switch Database

```sql
USE DATABASE SALES_DB;
```

Switch Schema

```sql
USE SCHEMA RAW;
```

---

# Complete Example

```sql
CREATE ROLE DATA_ENGINEER;

CREATE USER ANUSHA
PASSWORD='Password123';

GRANT ROLE DATA_ENGINEER
TO USER ANUSHA;

GRANT USAGE
ON DATABASE SALES_DB
TO ROLE DATA_ENGINEER;

GRANT USAGE
ON SCHEMA SALES_DB.RAW
TO ROLE DATA_ENGINEER;

GRANT SELECT, INSERT
ON TABLE SALES_DB.RAW.CUSTOMERS
TO ROLE DATA_ENGINEER;
```

---

# Best Practices

- Use roles instead of granting privileges directly to users.
- Follow the Principle of Least Privilege.
- Avoid using ACCOUNTADMIN for daily work.
- Grant only the permissions required.
- Use custom roles for different teams (Data Engineer, Analyst, BI Developer).
- Avoid assigning sensitive permissions to PUBLIC.

---

# Interview Questions

### What is RBAC?

Role-Based Access Control is a security model where permissions are assigned to roles and roles are assigned to users.

---

### Why is RBAC preferred?

- Easier administration
- Better security
- Reusable permissions
- Easier auditing

---

### Which role creates databases?

SYSADMIN

---

### Which role manages users?

USERADMIN

---

### Which role manages roles and privileges?

SECURITYADMIN

---

### Which role has the highest privileges?

ACCOUNTADMIN

---

### Can users have multiple roles?

Yes.

---

### Why is USAGE required?

Because access to a database and schema is required before accessing objects inside them.

---

### What is OWNERSHIP?

The OWNERSHIP privilege gives full control over an object, including the ability to grant privileges and transfer ownership.
