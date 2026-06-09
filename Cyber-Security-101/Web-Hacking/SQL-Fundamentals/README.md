# 🗄️ SQL Fundamentals

> **Room Path:** Cyber Security 101 → Web Hacking → SQL Fundamentals  
> **Status:** ✅ Completed

---

## 📌 Why It Matters

Databases are **everywhere** — Web Apps, SIEMs, Authentication Systems, Malware Analysis Tools.

| Side | Use Case |
|------|----------|
| ⚔️ **Offensive** | SQL Injection jaise vulnerabilities ko samjhna aur exploit karna — compromised services se data extract/tamper karna |
| 🛡️ **Defensive** | Suspicious activities find karna, restrictions implement karna, services ko attacks se protect karna |

---

## 🎯 Learning Objectives

| Topic | Description |
|-------|-------------|
| Database Basics | Key terms, concepts aur types of databases |
| What is SQL | Intro to Structural Query Language |
| CRUD Operations | Create, Read, Update, Delete |
| Clauses | WHERE, ORDER BY, GROUP BY |
| Operators | AND, OR, NOT, Comparison Operators |
| Functions | COUNT, SUM, AVG and more |

---

## 🗂️ Databases 101

> **Database** = Organized data jo easily accessible, manipulatable aur analyzable ho.  
> Example: TryHackMe apna username aur password database mein store karta hai.

### Two Main Types

| Feature | Relational (SQL) | Non-Relational (NoSQL) |
|---------|-----------------|----------------------|
| **Format** | Tables, Rows, Columns | Documents, Key-Value Pairs |
| **Data** | Structured, Consistent | Flexible, Varying Format |
| **Best For** | E-commerce, Authentication | Social Media, Scanned Documents |
| **Example** | User Login Data | MongoDB Document |

### 🔑 Primary Key
- Har record ko **uniquely identify** karta hai
- No two rows can have same value
- **Only one** Primary Key per table
- Example: Student ka Matriculation Number

### 🔗 Foreign Key
- Ek column jo **doosre table mein bhi exist** karta hai
- Do tables ke beech **relationship** create karta hai
- Example: `Authors_id` in Books table → links to `id` in Authors table
- A table can have **multiple** foreign keys

---

## 💬 What is SQL?

**DBMS (Database Management System)** — software jo tumhare aur database ke beech mein hota hai. Data retrieve, update aur manage karne deta hai.  
Examples: MySQL, MongoDB, Oracle Database, MariaDB

**SQL (Structured Query Language)** — relational database mein data query, define aur manipulate karne ki language.

### Why SQL is Worth It?

| Feature | Detail |
|---------|--------|
| ⚡ **Fast** | Massive amount of data almost instantly return karta hai |
| 📖 **Readable** | Plain English mein likha hota hai |
| ✅ **Reliable** | Strict structure data accuracy guarantee karta hai |
| 🔧 **Flexible** | Har tarah ke data analysis ke liye powerful querying |

### 🖥️ Connect to MySQL
```bash
mysql -u root -p
# Password: tryhackme
```

---

## 🏗️ Database & Table Statements

### Database Commands

| Command | Kya karta hai | Syntax |
|---------|--------------|--------|
| `CREATE DATABASE` | Naya database banao | `CREATE DATABASE name;` |
| `SHOW DATABASES` | Sab databases ki list | `SHOW DATABASES;` |
| `USE` | Database ko active karo | `USE name;` |
| `DROP DATABASE` | Database delete karo | `DROP DATABASE name;` |

### Table Commands

| Command | Kya karta hai |
|---------|--------------|
| `CREATE TABLE` | Naya table banao with columns & data types |
| `SHOW TABLES` | Active database ke tables list karo |
| `DESCRIBE` / `DESC` | Table ki columns aur data types dekho |
| `ALTER TABLE` | Table mein changes karo (add/rename/remove column) |
| `DROP TABLE` | Table delete karo |

### 📝 Practical Example — Creating a Table

```sql
CREATE TABLE book_inventory (
    book_id     INT AUTO_INCREMENT PRIMARY KEY,
    book_name   VARCHAR(255) NOT NULL,
    publish_date DATE
);
```

| Keyword | Matlab |
|---------|--------|
| `AUTO_INCREMENT` | ID automatically 1, 2, 3... assign hoti hai |
| `PRIMARY KEY` | Har record ka unique identifier |
| `VARCHAR(255)` | Text, max 255 characters |
| `NOT NULL` | Field empty nahi ho sakta |
| `DATE` | Date format |

---

## ✏️ CRUD Operations

> **CRUD** = Create, Read, Update, Delete

### ➕ Create — INSERT

```sql
INSERT INTO books (id, name, publish_date, description)
VALUES (1, 'English Book', '2025-06-24', 'This is all about Grammar');
```

### 📖 Read — SELECT

```sql
-- Sab columns
SELECT * FROM my_books;

-- Specific columns
SELECT name, description FROM my_books;
```

### 🔄 Update — UPDATE

```sql
UPDATE my_books
SET description = 'New description here'
WHERE id = 1;
```

### 🗑️ Delete — DELETE

```sql
DELETE FROM my_books WHERE id = 1;
```

---

## 📋 SQL Clauses

> **Clause** = Batata hai konsa data retrieve karo, kaise sort karo, kaise group karo.

### DISTINCT — Duplicates hatao

```sql
SELECT DISTINCT name FROM my_books;
```

### GROUP BY — Data group karo

```sql
SELECT name, COUNT(*)
FROM my_books
GROUP BY name;
```

### ORDER BY — Data sort karo

```sql
-- A to Z (Ascending)
SELECT * FROM my_books ORDER BY publish_date ASC;

-- Z to A (Descending)
SELECT * FROM my_books ORDER BY publish_date DESC;
```

### HAVING — GROUP BY ke baad filter karo

```sql
SELECT name, COUNT(*)
FROM books
GROUP BY name
HAVING name LIKE '%Hack%';
```

---

## ⚙️ SQL Functions

### String Functions — Text pe kaam karte hain

| Function | Kaam | Example |
|----------|------|---------|
| `CONCAT()` | Multiple strings jodo | `CONCAT(name, ' is a ', category)` |
| `GROUP_CONCAT()` | Multiple rows ka data ek field mein | `GROUP_CONCAT(name SEPARATOR ', ')` |
| `SUBSTRING()` | String ka ek hissa nikalo | `SUBSTRING(published_date, 1, 4)` |
| `LENGTH()` | String ke characters count karo | `LENGTH(name)` |

### Aggregate Functions — Numbers pe kaam karte hain

| Function | Kaam | Example |
|----------|------|---------|
| `COUNT()` | Total rows count karo | `COUNT(*)` |
| `SUM()` | Column ki values jodo | `SUM(price)` |
| `MAX()` | Sab se bara value | `MAX(published_date)` |
| `MIN()` | Sab se chhota value | `MIN(published_date)` |

---

*TryHackMe — SQL Fundamentals Room ✅*
