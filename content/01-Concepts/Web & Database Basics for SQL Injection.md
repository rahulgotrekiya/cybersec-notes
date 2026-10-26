---
type: concept
subject: ehpt
tags: [cybersecurity, concept, ehpt, web-security, http, database, prerequisite]
status: learning
created: 2026-09-10
---

# Web & Database Basics for SQL Injection

> 📝 **Concept Note** — EHPT | Prerequisite for [[SQL Injection]]

> This note explains the machinery a web application uses so you understand *where* SQL injection actually happens. Read [[SQL Essentials for SQL Injection]] alongside this, then move to [[SQL Injection]].

---

## The Problem It Solves

SQL injection is not magic and it is not "hacking the database directly". It is abuse of a **normal, everyday chain**: a user types into a web form, the browser sends it to a server, the server builds a SQL query using that input, and the database runs it. To attack this chain you must first see every link in it. This note draws that chain end to end.

---

## How a Web Application Works (the request-response cycle)

A web application has three tiers. Injection happens because **untrusted input from tier 1 reaches tier 3**.

```
   TIER 1: CLIENT          TIER 2: SERVER            TIER 3: DATABASE
   (your browser)          (PHP / Node / Java)       (MySQL / MSSQL)
        |                        |                          |
        |  1. HTTP request       |                          |
        | ---------------------> |                          |
        |  (form data, URL)      |  2. builds a SQL query   |
        |                        |  using your input        |
        |                        | -----------------------> |
        |                        |                          | 3. runs query
        |                        |  4. results              |
        |                        | <----------------------- |
        |  5. HTML response       |                          |
        | <--------------------- |                          |
```

**The key insight:** the database will faithfully execute *whatever query the server hands it*. If the server builds that query by gluing your input directly into it, then **you partly control the query** — and that is SQL injection.

---

## HTTP — How Input Reaches the Server

**HTTP (HyperText Transfer Protocol)** is how the browser talks to the server. Injectable input arrives through HTTP in several places.

### GET vs POST

| Method | Where data goes | Visible in | Typical use |
|--------|-----------------|-----------|-------------|
| **GET** | In the **URL** as query parameters | Address bar, logs, history | Search, links, filters |
| **POST** | In the **request body** | Not in the URL | Logins, forms, sensitive data |

**GET example — the parameter after `?` is user input:**
```
https://shop.com/product.php?id=5
                             ^^^^
                 "id" parameter = 5  (an attacker can change this to  5' OR '1'='1)
```

**POST example — data travels in the body:**
```
POST /login.php HTTP/1.1
Host: shop.com
Content-Type: application/x-www-form-urlencoded

username=rahul&password=secret123
```

### Every input point is a potential injection point
Anywhere the user can send data, an attacker can send a payload:
- URL query parameters (`?id=5`)
- Form fields (login, search, registration)
- HTTP headers (`User-Agent`, `Cookie`, `Referer`)
- Hidden fields and JSON API bodies

---

## Where the Database Fits In

Web apps do not store data in the page — they store it in a **database** and fetch it on each request. The **backend code** is the middleman that turns your input into a SQL query.

**A vulnerable login in PHP (this is the whole problem in four lines):**
```php
$user = $_POST['username'];
$pass = $_POST['password'];
$query = "SELECT * FROM users WHERE username = '$user' AND password = '$pass'";
$result = mysqli_query($conn, $query);
```

The developer **glued `$user` and `$pass` straight into the query string**. The database cannot tell which part came from the trusted developer and which part came from the untrusted user — to it, the finished string is just one query. That confusion is the root cause of SQL injection.

---

## Why It Is Dangerous: Data and Code on the Same Line

The fundamental flaw is that the query **mixes two different things in one string**:
- **Code** — the SQL the developer wrote (`SELECT ... WHERE username =`)
- **Data** — the value the user supplied (`rahul`)

When user data is concatenated into the code, a user who sends **SQL syntax instead of a plain value** can change the meaning of the code itself. The database has no way to know that `' OR '1'='1` was supposed to be "just a username".

```
   Intended:  SELECT * FROM users WHERE username = '[DATA]'
   Attack:    SELECT * FROM users WHERE username = '' OR '1'='1'
                                                    ^^^^^^^^^^^^^
                              user's "data" became part of the CODE
```

---

## Sessions, Cookies and Login State

Because HTTP is **stateless** (it does not remember previous requests), web apps track logged-in users with **sessions and cookies**. This matters for SQLi because:
- A successful injection that **bypasses the login** often causes the app to create a valid session for you as another user, typically **admin**.
- Stolen data frequently includes **session tokens** or **password hashes** from the database.

(See [[OSI Model & TCP-IP Security]] and the session/cookie material for the underlying mechanism.)

---

## Tools You Will Use

| Tool | Role in SQL injection |
|------|----------------------|
| **Web browser** | Manually change URL parameters to test | 
| **[[Burp Suite]]** | Intercept, modify and replay HTTP requests before they reach the server |
| **DVWA** | Damn Vulnerable Web Application — a safe, intentionally vulnerable practice target |
| **sqlmap** | Automated tool that detects and exploits SQL injection |
| **Developer Tools (F12)** | Inspect forms, parameters and requests in the browser |

---

## ⚠️ Prerequisites Checklist

Before attempting SQL injection you should be comfortable with:
- [ ] Basic SQL — `SELECT`, `WHERE`, quotes, `UNION`, comments (see [[SQL Essentials for SQL Injection]])
- [ ] The client → server → database request flow
- [ ] The difference between GET and POST, and where parameters live
- [ ] The idea that backend code builds a query from user input
- [ ] Why mixing code and data in one string is the root cause
- [ ] Using a browser and an intercepting proxy like [[Burp Suite]]

> ⚠️ **Ethics and legality:** Only ever test SQL injection on systems you own or are explicitly authorised to test, such as **DVWA** in an isolated lab. Unauthorised access to a computer system is a criminal offence.

---

## Related Concepts

- [[SQL Essentials for SQL Injection]] — the SQL syntax itself
- [[SQL Injection]] — the attack this note prepares you for
- [[Web Application Security]] — the wider category of web attacks
- [[Reconnaissance & Scanning]] — finding targets and input points
- [[Burp Suite]] — the proxy used to manipulate requests
