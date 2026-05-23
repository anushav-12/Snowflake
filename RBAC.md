# RBAC in Snowflake

## Simple Definition

RBAC stands for:

```text
Role-Based Access Control
```

It is a security mechanism in Snowflake where:
- Permissions are assigned to roles
- Roles are assigned to users

Instead of directly giving permissions to users.

---

# Basic Flow

```text
Privileges → Roles → Users
```

---

# Simple Example

Suppose:
- Analyst can only view data
- Developer can modify data

Then different roles are created for each.

---

# Create Role

```sql
CREATE ROLE analyst_role;
```

---

# Grant Permission to Role

```sql
GRANT SELECT ON TABLE employees TO ROLE analyst_role;
```

This means:
- analyst_role can read employees table

---

# Assign Role to User

```sql
GRANT ROLE analyst_role TO USER anusha;
```

Now:
- User anusha gets all permissions of analyst_role

---

# Important Components

## 1. User

Actual person or application using Snowflake.

### Example
- anusha
- admin_user

---

## 2. Role

Collection of permissions.

### Example
- analyst_role
- developer_role

---

## 3. Privilege

Specific permission.

### Examples
- SELECT
- INSERT
- UPDATE
- DELETE
- CREATE

---

## 4. Object

Things permissions are applied on.

### Examples
- Database
- Schema
- Table
- Warehouse
- View

---

# Built-in Roles in Snowflake

| Role | Purpose |
|---|---|
| ACCOUNTADMIN | Full access |
| SYSADMIN | Manage objects |
| SECURITYADMIN | Manage security and roles |
| USERADMIN | Manage users |
| PUBLIC | Default role for all users |

---

# Role Hierarchy

Snowflake supports role inheritance.

Example:

```text
ACCOUNTADMIN
    ↓
SYSADMIN
    ↓
CUSTOM_ROLE
```

Higher roles inherit permissions from lower roles.

---

# Why RBAC is Important

RBAC helps with:
- Security
- Access control
- Easier permission management
- Restricting unauthorized access

---

# Real-Time Example

| Team | Access |
|---|---|
| Data Analyst | Read-only access |
| Data Engineer | Read + Write access |
| Admin | Full access |

RBAC ensures each team gets only required access.

---

# Important Interview Points

- RBAC stands for Role-Based Access Control
- Permissions are assigned to roles
- Roles are assigned to users
- Improves security and management
- Snowflake provides built-in admin roles

---

# Quick Revision Notes

```text
Privileges → Roles → Users
```

```text
Role = Collection of permissions
```

```text
RBAC improves security and access management
```

---

# What I Learned

- Snowflake uses RBAC for security
- Users get permissions through roles
- Roles simplify permission management
- Built-in admin roles already exist
- Access can be controlled efficiently
