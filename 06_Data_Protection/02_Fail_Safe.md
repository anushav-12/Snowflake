# 🛡️ Snowflake Fail-safe

## What is Fail-safe?

**Fail-safe** is a Snowflake data recovery feature that protects data **after the Time Travel retention period expires**.

It provides an additional **7-day recovery window** for disaster recovery.

> **One-Line Definition:**  
> Fail-safe is a 7-day recovery period after Time Travel, where only Snowflake Support can recover data.

---

# Why is Fail-safe Needed?

Fail-safe acts as the **last layer of protection** if data cannot be recovered using Time Travel.

### Example

A table was accidentally dropped.

- Time Travel retention has already expired.
- User can no longer recover it using `UNDROP`.

The data enters **Fail-safe**, where **Snowflake Support** may be able to recover it.

---

# How Fail-safe Works

```
Data Created
      │
      ▼
Time Travel
(0–90 Days)
      │
      ▼
Fail-safe
(7 Days)
      │
      ▼
Permanent Deletion
```

Once the 7-day Fail-safe period ends, the data is permanently deleted.

---

# Key Features

- Automatically enabled by Snowflake
- Starts **after Time Travel ends**
- Lasts for **7 days**
- Users cannot query or restore data
- Recovery is handled only by **Snowflake Support**
- Used for disaster recovery

---

# How Recovery Works

Suppose a table is deleted.

### During Time Travel

Recover using:

```sql
UNDROP TABLE employee;
```

or

```sql
SELECT *
FROM employee
AT(OFFSET => -3600);
```

### During Fail-safe

❌ Users cannot run SQL commands.

Recovery requires contacting **Snowflake Support**.

---

# Objects Supporting Fail-safe

| Object | Supported |
|---------|-----------|
| Permanent Tables | ✅ |
| Databases | ✅ |
| Schemas | ✅ |
| Transient Tables | ❌ |
| Temporary Tables | ❌ |

> **Note:** Fail-safe is **not available** for Temporary or Transient tables.

---

# Time Travel vs Fail-safe

| Feature | Time Travel | Fail-safe |
|---------|-------------|-----------|
| User Accessible | ✅ Yes | ❌ No |
| SQL Queries Allowed | ✅ Yes | ❌ No |
| UNDROP Supported | ✅ Yes | ❌ No |
| Duration | 0–90 Days | 7 Days |
| Purpose | Recover user mistakes | Disaster recovery |
| Recovery By | User | Snowflake Support |

---

# Fail-safe vs Backup

| Fail-safe | Traditional Backup |
|------------|-------------------|
| Automatic | Manual/Scheduled |
| Fixed 7 Days | User-defined |
| No SQL Access | Full Access |
| Not User Controlled | User Controlled |
| Disaster Recovery | Long-term Backup |

---

# Limitations

- Cannot be disabled for permanent objects.
- Users cannot access data directly.
- No SQL commands work during Fail-safe.
- Recovery is not guaranteed for every situation.
- Not a replacement for backups.

---

# Real Project Example

### Scenario

A production table was accidentally dropped.

```sql
DROP TABLE SALES_TRANSACTION;
```

The Time Travel retention has already expired.

### Result

- ❌ `UNDROP TABLE` no longer works.
- ❌ Historical queries are unavailable.
- ✅ Contact Snowflake Support for recovery during the 7-day Fail-safe period.

---

# Interview Questions

### What is Fail-safe?

Fail-safe is a **7-day recovery period** after Time Travel expires, used for disaster recovery.

---

### Can users access Fail-safe data?

**No.** Only Snowflake Support can recover data during Fail-safe.

---

### Can we query data in Fail-safe?

**No.** SQL queries are not allowed.

---

### Does every table have Fail-safe?

**No.**

- Permanent Tables → ✅ Yes
- Temporary Tables → ❌ No
- Transient Tables → ❌ No

---

### What happens after Fail-safe?

The data is **permanently deleted** and cannot be recovered.

---

# Quick Revision

- ✅ Starts after Time Travel
- ✅ Duration: 7 Days
- ✅ Disaster Recovery
- ✅ Snowflake Support Only
- ✅ No SQL Access
- ✅ No UNDROP
- ✅ Permanent Tables Only
- ❌ Not a Backup

---

## ⭐ One-Line Summary

**Fail-safe is Snowflake's automatic 7-day disaster recovery period after Time Travel expires, where only Snowflake Support can recover data.**
