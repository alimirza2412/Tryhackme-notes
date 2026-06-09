# 🗄️ SQL Fundamentals — Complete Notes

> **Databases are everywhere** — Web Apps, SIEMs, Authentication Systems, Malware Analysis Tools.  
> These notes cover both offensive and defensive perspectives. 💻🔐

---

## 📌 Why It Matters?

| Side | Use Case |
|------|----------|
| ⚔️ **Offensive** | Understanding and exploiting SQL injection vulnerabilities... extracting or tampering data from compromised services |
| 🛡️ **Defensive** | Finding suspicious activities in databases, implementing restrictions, and protecting services from attacks |

---

## 🎯 Learning Objectives

| Topic | Description |
|-------|-------------|
| 📦 Database Basics | Key terms, concepts and types of databases |
| 🔤 What is SQL | Intro to Structured Query Language |
| ⚙️ CRUD Operations | Create, Read, Update and Delete data |
| 📋 Clauses | WHERE, ORDER BY, GROUP BY |
| 🔧 Operators | AND, OR, NOT, Comparison Operators |
| 📊 Functions | COUNT, SUM, AVG and more |

---

## 🗃️ Databases 101

> **Definition:** Databases are organized data that can be easily accessed, manipulated and analyzed.  
> **Example:** TryHackMe stores your username and passwords in a database.

### Two Main Types

| Feature | Relational (SQL) | Non-Relational (NoSQL) |
|---------|-----------------|----------------------|
| **Format** | Tables, Rows, Columns | Documents, Key-Value Pairs |
| **Data** | Structured, Consistent | Flexible, Varying Format |
| **Best For** | E-commerce, Authentication | Social Media, Scanned Documents |
| **Example** | User Login Data | MongoDB Document |

---

## 🔑 Key Concepts

### Primary Key
- Uniquely identifies every record in a table
- No two rows can have the same value
- **Example:** A student's matriculation number
- ⚠️ Only **one** Primary Key per table

### Foreign Key
- A column that also exists in another table
- Creates a relationship between two tables
- **Example:** `author_id` in the books table links to `id` in the authors table
- ✅ A table can have **multiple** foreign keys

---

## 💻 What is SQL?

**DBMS (Database Management System)** — software that sits between you and the database. It lets you retrieve, update and manage data.

> **Examples:** MongoDB, MySQL, Oracle Database, MariaDB

**SQL — Structured Query Language** *(A programming language used to query, define and manipulate data in a relational database)*

### Why SQL is Worth It?

| Feature | Description |
|---------|-------------|
| ⚡ **Fast** | Returns massive amounts of data almost instantly |
| 📖 **Readable** | Written in plain English, easy to understand |
| ✅ **Reliable** | Strict structure guarantees data accuracy |
| 🔀 **Flexible** | Powerful querying for all kinds of data analysis |

---

## 🛠️ Hands-On SQL

```bash
mysql -u root -p
# Password: tryhackme
```

---

## 📂 Database & Table Statements

### Database Commands

| Command | What it Does | Syntax |
|---------|-------------|--------|
| `CREATE DATABASE` | Create a new database | `CREATE DATABASE name;` |
| `SHOW DATABASES` | List all databases | `SHOW DATABASES;` |
| `USE` | Set a database as active | `USE name;` |
| `DROP DATABASE` | Delete a database | `DROP DATABASE name;` |

### Table Commands

| Command | What it Does |
|---------|-------------|
| `CREATE TABLE` | Create a new table with columns & data types |
| `SHOW TABLES` | List all tables in the active database |
| `DESCRIBE` / `DESC` | View a table's columns and data types |
| `ALTER TABLE` | Modify a table (add/rename/remove column) |
| `DROP TABLE` | Delete a table |

### Practical Example — Creating a Table

```sql
CREATE TABLE book_inventory (
    book_id      INT AUTO_INCREMENT PRIMARY KEY,
    book_name    VARCHAR(255) NOT NULL,
    publish_date DATE
);
```

**Breaking it down:**
- `AUTO_INCREMENT` → ID is automatically assigned as 1, 2, 3...
- `PRIMARY KEY` → Unique identifier for each record
- `VARCHAR(255)` → Text field, max 255 characters
- `NOT NULL` → Field cannot be left empty
- `DATE` → Date format

---

## ⚙️ CRUD Operations

> **CRUD = Create, Read, Update, Delete**

### Create (INSERT)
```sql
INSERT INTO books (id, name, publish_date, description)
VALUES (1, 'English Book', '2025-06-24', 'This is all about Grammar');
```

### Read (SELECT)
```sql
-- All columns
SELECT * FROM my_books;

-- Specific columns
SELECT name, description FROM my_books;
```

### Update (UPDATE)
```sql
UPDATE my_books
SET description = 'New description here'
WHERE id = 1;
```

### Delete (DELETE)
```sql
DELETE FROM my_books WHERE id = 1;
```

---

## 📋 SQL Clauses

> Clauses define criteria — which data to retrieve, how to sort it, how to group it.

### DISTINCT — Remove duplicates
```sql
SELECT DISTINCT name FROM my_books;
```

### GROUP BY — Group data
```sql
SELECT name, COUNT(*)
FROM my_books
GROUP BY name;
```

### ORDER BY — Sort data
```sql
-- Ascending (A to Z)
SELECT * FROM my_books ORDER BY publish_date ASC;

-- Descending (Z to A)
SELECT * FROM my_books ORDER BY publish_date DESC;
```

### HAVING — Filter after GROUP BY
```sql
SELECT name, COUNT(*)
FROM books
GROUP BY name
HAVING name LIKE '%Hack%';
```

---

## 📊 SQL Functions

### String Functions — Work on text

| Function | What it Does | Example |
|----------|-------------|---------|
| `CONCAT()` | Join multiple strings together | `CONCAT(name, ' is a ', category)` |
| `GROUP_CONCAT()` | Combine multiple rows into one field | `GROUP_CONCAT(name SEPARATOR ', ')` |
| `SUBSTRING()` | Extract part of a string | `SUBSTRING(published_date, 1, 4)` |
| `LENGTH()` | Count characters in a string | `LENGTH(name)` |

### Aggregate Functions — Work on numbers

| Function | What it Does | Example |
|----------|-------------|---------|
| `COUNT()` | Count total rows | `COUNT(*)` |
| `SUM()` | Add up column values | `SUM(price)` |
| `MAX()` | Get the largest value | `MAX(published_date)` |
| `MIN()` | Get the smallest value | `MIN(published_date)` |

---

## 🚀 Quick Reference Cheatsheet

```sql
-- Database setup
CREATE DATABASE mydb;
USE mydb;

-- Create a table
CREATE TABLE users (
    id       INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) NOT NULL,
    password VARCHAR(255) NOT NULL
);

-- Insert data
INSERT INTO users (username, password) VALUES ('admin', 'secret123');

-- Read data
SELECT * FROM users;

-- Update data
UPDATE users SET password = 'newpass' WHERE id = 1;

-- Delete data
DELETE FROM users WHERE id = 1;
```

---

> 📝 *Notes — Learning SQL for both offensive and defensive cybersecurity* 🔐
