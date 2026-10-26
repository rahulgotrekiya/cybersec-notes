---
type: concept
subject: ehpt
tags: [cybersecurity, concept, ehpt, sql, database, web-security, prerequisite]
status: learning
created: 2026-09-10
---

# SQL Essentials for SQL Injection

> 📝 **Concept Note** — EHPT | Prerequisite for [[SQL Injection]]

> This note covers only the SQL you actually need to understand and perform SQL injection. It is not a full database course. Read this first, then [[Web & Database Basics for SQL Injection]], then [[SQL Injection]].

---

## The Problem It Solves

You cannot inject SQL if you do not understand SQL. Every injection payload is just **valid SQL** inserted where the developer expected plain data. To read a payload like `' OR '1'='1' -- ` and know *why* it works, you must first understand how a normal `SELECT` query is built and evaluated. This note gives you exactly that foundation.

---

## What is SQL?

**SQL (Structured Query Language)** is the standard language used to communicate with a **relational database**. A web application uses SQL behind the scenes to store and retrieve data — user accounts, products, orders, comments.

Data is stored in **tables**, which are made of **rows** (records) and **columns** (fields).

```
              TABLE: users
   +----+----------+------------------+-------+
   | id | username | password         | role  |   <- columns
   +----+----------+------------------+-------+
   | 1  | admin    | 5f4dcc3b5aa...   | admin |   <- row (record)
   | 2  | rahul    | e10adc3949ba...  | user  |
   | 3  | john     | 25d55ad283aa...  | user  |
   +----+----------+------------------+-------+
```

---

## The SELECT Statement (the most important one)

`SELECT` retrieves data. The vast majority of SQL injection happens through `SELECT` queries because that is what login pages, search boxes and product pages run.

```sql
SELECT column1, column2 FROM table_name WHERE condition;
```

| Clause | Meaning |
|--------|---------|
| `SELECT` | Which columns to return |
| `FROM` | Which table to read from |
| `WHERE` | The condition a row must satisfy to be returned |
| `*` | Shorthand meaning "all columns" |

**Examples:**
```sql
SELECT * FROM users;                          -- every column, every row
SELECT username FROM users WHERE id = 2;      -- one column, one matching row
SELECT * FROM users WHERE role = 'admin';     -- only admin rows
```

---

## The WHERE Clause and Boolean Logic

This is the heart of SQL injection. The `WHERE` clause is a **condition that is either TRUE or FALSE** for each row. Only rows where it is TRUE are returned.

```sql
SELECT * FROM users WHERE username = 'rahul' AND password = 'secret123';
```

- If a row makes the condition **TRUE**, it is returned, and login succeeds.
- If no row matches, **nothing** is returned, and login fails.

### Logical operators

| Operator | Meaning | Example condition | Result |
|----------|---------|-------------------|--------|
| `AND` | Both sides must be true | `role='admin' AND id=1` | true only if both hold |
| `OR` | Either side being true is enough | `id=1 OR 1=1` | **always true**, because `1=1` is always true |
| `=` | Equality comparison | `username = 'admin'` | true when they match |

> **The single most important fact for SQL injection:** `OR 1=1` makes a `WHERE` clause **always true**, so the database returns **every row**. This is why authentication can be bypassed.

---

## String Literals and Quotes

Text values in SQL must be wrapped in **single quotes**:

```sql
SELECT * FROM users WHERE username = 'rahul';
```

The quotes tell the database "everything between me is **data**, not a command". SQL injection works by **breaking out of these quotes** so the database treats attacker input as **commands** instead of data. Understanding this one point explains the entire vulnerability.

---

## SQL Comments

Comments make the database **ignore the rest of a line**. Attackers use them to cut off the part of the original query they do not want to run.

| Syntax | Database |
|--------|----------|
| `-- ` (two dashes + a space) | Standard SQL, MySQL, MSSQL, PostgreSQL |
| `#` | MySQL |
| `/* ... */` | Inline / multi-line comment |

**Example:** the payload `admin' -- ` turns the rest of the query into a comment, so any password check after it is discarded.

---

## The UNION Operator

`UNION` combines the results of **two** `SELECT` statements into one result set. It is the basis of **UNION-based SQL injection**, where an attacker appends their own `SELECT` to steal data from other tables.

```sql
SELECT name, price FROM products
UNION
SELECT username, password FROM users;
```

**Two strict rules for UNION (and therefore for the attack):**
1. Both `SELECT`s must return the **same number of columns**.
2. The corresponding columns must have **compatible data types**.

This is why an attacker first has to *find the number of columns* before a UNION injection will work.

---

## Useful Functions and Metadata

Databases expose information about themselves, which attackers read to understand the target.

| Query | Returns |
|-------|---------|
| `SELECT version();` | The database software and version |
| `SELECT database();` | The current database name |
| `SELECT user();` | The database user the app connects as |
| `information_schema.tables` | A list of all table names |
| `information_schema.columns` | A list of all column names |

The **`information_schema`** is a built-in database that describes every other database. Attackers query it to discover table and column names before extracting the actual data.

---

## Other Statements to Recognise

You mostly attack `SELECT`, but you should recognise these because they appear in more damaging injections:

| Statement | Purpose | Danger if injected |
|-----------|---------|--------------------|
| `INSERT` | Add a new row | Create a rogue admin account |
| `UPDATE` | Modify existing rows | Change any user's password |
| `DELETE` | Remove rows | Destroy data |
| `DROP TABLE` | Delete an entire table | Wipe out a table completely |

The classic joke payload `'; DROP TABLE students; -- ` is a **stacked query** that tries to run a second, destructive statement.

---

## ⚠️ Key Takeaways for Injection

- Web apps build SQL queries and send them to a database; `SELECT ... WHERE` powers logins and searches.
- The `WHERE` clause is TRUE/FALSE logic — and `OR 1=1` forces it always TRUE.
- Text data lives inside **single quotes**; injection escapes those quotes to inject commands.
- `-- ` and `#` comment out the rest of a query.
- `UNION` needs a matching column count, which is why attackers enumerate columns first.
- `information_schema` reveals all table and column names.

---

## Related Concepts

- [[Web & Database Basics for SQL Injection]] — how these queries reach the database
- [[SQL Injection]] — the attack that abuses everything above
- [[Web Application Security]] — where SQLi sits in the bigger picture
- [[OWASP Top 10 Quick Reference]] — injection is a Top 10 category ⚡
