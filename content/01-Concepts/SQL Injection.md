---
type: concept
subject: ehpt
tags: [cybersecurity, concept, ehpt, sql-injection, web-security, owasp, injection]
status: learning
created: 2026-09-10
---

# SQL Injection (SQLi)

> 📝 **Concept Note** — EHPT | Source: MCA Sem III Syllabus

> Read the two prerequisites first: [[SQL Essentials for SQL Injection]] (the SQL you need) and [[Web & Database Basics for SQL Injection]] (how input reaches the database).

---

## The Problem It Solves (for the attacker)

SQL Injection is one of the oldest and most damaging web vulnerabilities, and it sits in the **OWASP Top 10** under *Injection*. It lets an attacker interfere with the queries an application makes to its database — reading data they should not see, bypassing authentication, modifying or deleting data, and sometimes taking over the whole server. It happens because the application trusts user input enough to place it directly inside a SQL query.

---

## What is SQL Injection?

**Definition:** SQL Injection is a web security vulnerability in which an attacker inserts (injects) malicious SQL code into an application's input, causing the database to execute unintended commands.

The root cause is a **failure to separate code from data**: the developer builds a SQL query by concatenating untrusted user input directly into the query string, so the attacker's input is interpreted as **SQL commands** rather than as plain **data**.

---

## ⚙️ How SQL Injection Actually Works

Consider this common vulnerable login query, where `$user` and `$pass` come straight from the user:

```sql
SELECT * FROM users WHERE username = '$user' AND password = '$pass';
```

### Normal (safe) input
The user enters username `rahul` and password `secret123`:
```sql
SELECT * FROM users WHERE username = 'rahul' AND password = 'secret123';
```
The database looks for a row matching both. Fair enough.

### Malicious input — authentication bypass
Now the attacker enters the username:  `admin' -- `  and any password.
The query becomes:
```sql
SELECT * FROM users WHERE username = 'admin' -- ' AND password = 'anything';
```

Step by step:
1. The attacker's `'` **closes** the username string early.
2. `admin` is now the complete username value being searched for.
3. `-- ` turns the **rest of the line into a comment**, so the entire password check is ignored.
4. The query effectively becomes: `SELECT * FROM users WHERE username = 'admin'`.
5. The row for `admin` is returned, the app believes the login succeeded, and the attacker is **logged in as admin without a password**.

### Malicious input — the classic `OR 1=1`
If the attacker enters the username:  `' OR '1'='1' -- `
```sql
SELECT * FROM users WHERE username = '' OR '1'='1' -- ' AND password = '';
```
Because `'1'='1'` is **always true**, the `WHERE` clause is true for **every row**. The query returns all users, and the application typically logs the attacker in as the **first user in the table** — usually the administrator.

> This is why understanding the `WHERE` clause and `OR 1=1` from [[SQL Essentials for SQL Injection]] is essential: the whole attack is just valid SQL abusing that logic.

---

## Types of SQL Injection

### 1. In-Band SQLi (results come back on the same channel)
The most common and easiest, because the attacker sees the result directly in the web page.

**(a) Error-Based**
The attacker deliberately causes a database **error message**, and the app displays it. These messages leak the database version, table names, or even query data. Relies on the application showing detailed errors.

**(b) UNION-Based**
The attacker uses the `UNION` operator to append a second `SELECT` and pull data from other tables into the visible results.

```sql
-- original:
SELECT name, price FROM products WHERE id = 5;
-- injected id value:  5 UNION SELECT username, password FROM users -- 
SELECT name, price FROM products WHERE id = 5
UNION SELECT username, password FROM users -- ;
```
The usernames and passwords appear where product names and prices normally would. The attacker must first **find the number of columns** (using `ORDER BY n` or `UNION SELECT NULL, NULL, ...`) because UNION requires matching column counts.

### 2. Blind SQLi (no data or errors are shown)
The application is vulnerable, but it does not display query results or error messages. The attacker infers data **indirectly**, one true/false answer at a time.

**(a) Boolean-Based (Content-Based)**
The attacker sends a condition and watches how the **page changes**.
```
?id=5 AND 1=1     -> page loads normally  (condition TRUE)
?id=5 AND 1=2     -> page looks different  (condition FALSE)
```
By asking many true/false questions (for example "is the first letter of the admin password greater than 'm'?"), the attacker extracts data character by character.

**(b) Time-Based**
When even the page does not visibly change, the attacker asks the database to **wait** if a condition is true, and measures the response time.
```sql
?id=5 AND IF(1=1, SLEEP(5), 0) -- 
```
A 5-second delay means the condition was true. Slow, but it works when nothing else is visible.

### 3. Out-of-Band SQLi
Rare. The result is retrieved through a **different channel**, for example forcing the database to make a DNS or HTTP request to a server the attacker controls. Used when in-band and blind methods are not possible.

| Type | Results visible? | Speed | Difficulty |
|------|------------------|-------|-----------|
| Error-based | Yes (in errors) | Fast | Easy |
| UNION-based | Yes (in page) | Fast | Easy-Medium |
| Boolean blind | No (infer from page) | Slow | Medium |
| Time-based blind | No (infer from delay) | Very slow | Medium-Hard |
| Out-of-band | Via external channel | Varies | Hard |

---

## What an Attacker Can Achieve (Impact)

| Impact | Example |
|--------|---------|
| **Authentication bypass** | Log in as admin without a password |
| **Data theft** | Dump usernames, password hashes, emails, card data |
| **Data tampering** | Change prices, balances, grades, or any user's password |
| **Data destruction** | `DELETE` rows or `DROP` entire tables |
| **Privilege escalation** | Turn a normal account into an administrator |
| **Full system compromise** | With high DB privileges, read/write files or run OS commands |

A single SQLi flaw maps directly to all three parts of the [[CIA Triad]] — Confidentiality (data theft), Integrity (tampering) and Availability (deletion).

---

## A Typical Attack Methodology

1. **Find input points** — URL parameters, forms, headers (see [[Web & Database Basics for SQL Injection]]).
2. **Test for the flaw** — submit a single quote `'` and watch for an error or changed behaviour.
3. **Determine the type** — in-band, blind boolean, or time-based.
4. **Enumerate the database** — find the number of columns, then read `information_schema` for table and column names.
5. **Extract data** — use UNION or blind techniques to dump the target table (for example `users`).
6. **Escalate** — crack stolen password hashes, log in, or attempt deeper access.

> Tools such as **sqlmap** automate steps 2 to 6, but you must understand the manual method first.

---

## 🛡️ Prevention and Defence

SQL injection is almost entirely preventable. In order of importance:

### 1. Prepared Statements / Parameterised Queries (the primary fix)
The query structure is defined **first**, with placeholders, and user input is sent **separately** as data. The database therefore never treats input as code.

```php
// Safe: input can never change the query structure
$stmt = $conn->prepare("SELECT * FROM users WHERE username = ? AND password = ?");
$stmt->bind_param("ss", $user, $pass);
$stmt->execute();
```
Here `admin' -- ` is searched for as a literal username; it cannot break out because it is bound as **data**.

### 2. Input Validation (defence in depth)
Allow-list what input is acceptable — for example an `id` field should be numeric only. Reject anything else. This supports, but does not replace, prepared statements.

### 3. Least Privilege
The database account the web app uses should have **only** the permissions it needs. A read-only app account cannot `DROP` a table even if injected.

### 4. Escaping (a weaker, last-resort measure)
Escaping special characters helps but is error-prone and should never be the only defence.

### 5. Other layers
- **Stored procedures** (when written safely with parameters)
- A **Web Application Firewall (WAF)** to filter known payloads
- **Disable detailed error messages** in production so error-based SQLi leaks nothing
- Keep the database and framework **patched**

| Defence | Strength |
|---------|----------|
| Prepared statements / parameterised queries | **Primary, strongest** |
| Input validation (allow-list) | Strong support |
| Least privilege DB account | Limits damage |
| WAF | Extra layer, bypassable |
| Escaping only | Weak, error-prone |

---

## ⚠️ Gotchas / Exam Traps

- The **root cause** is mixing code and data in one query string, not "a weak password" or "an open port".
- `-- ` (comment) needs a **trailing space** in MySQL to work.
- **Blind SQLi still fully compromises data** even though nothing is displayed — it just takes longer.
- **UNION requires an equal column count and compatible types**, which is why column enumeration comes first.
- The correct primary defence is **parameterised queries**, not input validation or a WAF (those are secondary).
- Detailed database error messages should be **disabled in production**.

---

## 📺 Learn by Video
*Supplementary only — the technical content above is written independently and stands on its own.*

- Manual SQL injection walkthrough on DVWA (search) - [search](https://www.youtube.com/results?search_query=DVWA+SQL+injection+manual+walkthrough)
- Prepared statements vs SQL injection explained (search) - [search](https://www.youtube.com/results?search_query=prepared+statements+prevent+sql+injection)

---

## Related Concepts

- [[SQL Essentials for SQL Injection]] — the SQL syntax behind every payload
- [[Web & Database Basics for SQL Injection]] — how input reaches the database
- [[Web Application Security]] — SQLi within the OWASP Top 10
- [[Lab - SQL Injection on DVWA]] — hands-on practical
- [[Burp Suite]] — intercepting and modifying requests
- [[OWASP Top 10 Quick Reference]] — Injection category ⚡
- [[CIA Triad]] — what SQLi violates
