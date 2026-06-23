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

*Source: [PortSwigger Web Academy – SQL Injection](https://portswigger.net/web-security/sql-injection)*
