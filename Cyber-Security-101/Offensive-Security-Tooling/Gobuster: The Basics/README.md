# 🔍 Gobuster — Content Discovery & Enumeration

> **Room:** [Gobuster: The Basics](https://tryhackme.com/room/gobusterthebasics) | **Platform:** TryHackMe  
> **Difficulty:** Easy | **Category:** Web Enumeration / Reconnaissance  
> **Completed by:** [Muhammad Ali](https://tryhackme.com/p/Muhammad.Ali12)

---

## 📑 Table of Contents

- [What is Gobuster?](#-what-is-gobuster)
- [Real-Life Analogy](#-real-life-analogy)
- [3 Main Modes](#-3-main-modes)
- [Key Flags & Options](#-key-flags--options)
- [Lab Setup](#-lab-setup)
- [Task 1 — Directory Enumeration](#-task-1--directory-enumeration-dir-mode)
- [Task 2 — File Extension Enumeration](#-task-2--file-extension-enumeration)
- [Task 3 — Subdomain Enumeration (DNS Mode)](#-task-3--subdomain-enumeration-dns-mode)
- [Task 4 — Virtual Host Enumeration (VHost Mode)](#-task-4--virtual-host-enumeration-vhost-mode)
- [HTTP Status Codes Cheatsheet](#-http-status-codes-cheatsheet)
- [Concepts Learned](#-concepts-learned)

---

## 🤔 What is Gobuster?

**Gobuster** is a command-line tool that performs **brute-force enumeration** against websites. It takes a wordlist and tries every word against the target to discover hidden directories, files, subdomains, and virtual hosts.

```
Website  ──►  Gobuster  ──►  Tries 218,000+ names  ──►  Reports what exists
```

It doesn't "hack" in one step — it does **reconnaissance**, the very first phase of penetration testing.

---

## 🏢 Real-Life Analogy

> Imagine you want to explore a building you're not supposed to enter.

```
🏢 Building = Website
🚪 Doors    = Directories / Pages
🪟 Windows  = Hidden files
🚶 You      = Gobuster
📋 Your map = Wordlist
```

You check every door, every window, every exit — **systematically**.  
Gobuster does the exact same thing with a website, trying thousands of paths in seconds.

---

## 🛠️ 3 Main Modes

| Mode | What It Finds | Real-Life Analogy |
|------|--------------|-------------------|
| `dir` | Hidden pages & folders | Finding secret rooms in a building |
| `dns` | Subdomains | Finding all branches of a company |
| `vhost` | Virtual hosts on same IP | Finding multiple offices in one building |

### 1️⃣ Directory Mode (`dir`)

```bash
gobuster dir -u http://example.com -w wordlist.txt
```

Tries paths like `/admin`, `/backup`, `/secret` — things not linked anywhere on the site.

### 2️⃣ DNS Mode (`dns`)

```bash
gobuster dns -d example.com -w wordlist.txt
```

Discovers subdomains like `dev.example.com`, `admin.example.com`.

> ⚠️ **Why it matters:**  
> `tryhackme.com` → patched ✅  
> `mobile.tryhackme.com` → might be vulnerable ⚠️  
> `admin.tryhackme.com` → might be exposed ⚠️

### 3️⃣ VHost Mode (`vhost`)

```bash
gobuster vhost -u http://example.com -w wordlist.txt
```

> 💡 **DNS Mode vs VHost Mode:**  
> DNS mode queries actual DNS records. VHost mode sends HTTP requests with different `Host:` headers to find **hidden virtual hosts** that share the same IP but respond to different domain names — even if DNS doesn't list them.

```
                    ┌─────────────────────────┐
Same IP Address ──► │      Web Server         │
10.10.10.10         │  blog.example.thm  ✅   │
                    │  shop.example.thm  ✅   │
                    │  secret.example.thm ✅  │
                    └─────────────────────────┘
```

---

## 🚩 Key Flags & Options

| Flag | Meaning | Example |
|------|---------|---------|
| `-u` | Target URL | `-u http://example.com` |
| `-w` | Wordlist path | `-w /usr/share/wordlists/dirbuster/...` |
| `-t` | Threads (speed) | `-t 50` |
| `-x` | File extensions | `-x php,js,html` |
| `-d` | Domain (DNS mode) | `-d example.com` |
| `--domain` | Domain (VHost mode) | `--domain example.thm` |
| `--append-domain` | Append domain to wordlist | `--append-domain` |
| `--exclude-length` | Filter out responses by size | `--exclude-length 250-320` |

---

## 🧪 Lab Setup

Target machine was `offensivetools.thm`. DNS wasn't resolving properly due to an IPv6 (`::1`) issue.

**Fix — manually mapped the domain:**

```bash
echo "10.113.166.64 www.offensivetools.thm offensivetools.thm" >> /etc/hosts
```

This tells the system to resolve the domain directly to the target IP, bypassing DNS entirely.

---

## 📁 Task 1 — Directory Enumeration (dir mode)

**Goal:** Find hidden directories on `www.offensivetools.thm`

```bash
gobuster dir -u "www.offensivetools.thm" \
  -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt \
  -t 50
```

**Results:**

![Gobuster dir mode results](https://github.com/alimirza2412/Tryhackme-notes/blob/68a7fd4667e6f9b4f130cbe78e63b889e39634a3/Cyber-Security-101/Offensive-Security-Tooling/Gobuster%3A%20The%20Basics/gobuster%20task%201.png)

| Directory | Status | Notes |
|-----------|--------|-------|
| `/home` | 200 ✅ | Accessible page |
| `/libraries` | 403 ⛔ | Forbidden — exists but blocked |
| `/secret` | 301 🔀 | Redirect — **interesting!** |
| `/media`, `/templates`, `/modules`... | 301 | Standard redirects |

> 🎯 **Answer:** `/secret` — stands out immediately. Most directories are boilerplate CMS folders; `/secret` is not.

---

## 🗂️ Task 2 — File Extension Enumeration

**Goal:** Find a `.js` file inside the `/secret` directory

```bash
gobuster dir -u "www.offensivetools.thm/secret" \
  -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt \
  -x .js
```

**The `-x .js` flag** makes Gobuster try every wordlist entry **twice**:
- `/word` (directory)
- `/word.js` (file with extension)

**Results:**

![Gobuster extension scan finding flag.js](https://github.com/alimirza2412/Tryhackme-notes/blob/f7ff9f7427890452580b4f70486d0247cc9f6c21/Cyber-Security-101/Offensive-Security-Tooling/Gobuster%3A%20The%20Basics/gobuster%20task%202.png)

![Gobuster finished and curl command](https://github.com/alimirza2412/Tryhackme-notes/blob/2e7f02cfa3ed0cbac26b7d56494abbf83ac18a07/Cyber-Security-101/Offensive-Security-Tooling/Gobuster%3A%20The%20Basics/gobuster%20task%203.png)

| Path | Status | Notes |
|------|--------|-------|
| `/content` | 301 | Redirect |
| `/uploads` | 301 | Redirect |
| `/flag.js` | **200 ✅** | **Found it!** |

**Fetched the flag using curl:**

```bash
curl www.offensivetools.thm/secret/flag.js
```

```
THM{ReconWasASuccess}
```

> 🏁 **Flag:** `THM{ReconWasASuccess}`

---

## 🌐 Task 3 — Subdomain Enumeration (DNS Mode)

**Goal:** Discover subdomains of `offensivetools.thm`

```bash
gobuster dns -d offensivetools.thm \
  -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

**Subdomains discovered:**

```
shop.offensivetools.thm    ✅
blog.offensivetools.thm    ✅
admin.offensivetools.thm   ✅
```

Each subdomain is a separate attack surface — a forgotten subdomain can be running outdated software or have weaker security than the main site.

---

## 🖥️ Task 4 — Virtual Host Enumeration (VHost Mode)

**Goal:** Find hidden virtual hosts on the same server IP

```bash
gobuster vhost -u "http://10.113.166.64" \
  --domain offensivetools.thm \
  -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt \
  --append-domain \
  --exclude-length 250-320
```

> **`--exclude-length 250-320`** — Filters out false positives. The server was returning ~250-320 byte responses for non-existent vhosts (default error pages). We exclude those to see only real results.

**Results:**

![Gobuster vhost enumeration results](https://github.com/alimirza2412/Tryhackme-notes/blob/ae383cc4d68e9286fc4a9aaf4b9c724ca48e7a7b/Cyber-Security-101/Offensive-Security-Tooling/Gobuster%3A%20The%20Basics/gobuster%20task%204.png)

| Virtual Host | Status | Size |
|-------------|--------|------|
| `forum.offensivetools.thm` | 200 ✅ | 2635 |
| `store.offensivetools.thm` | 200 ✅ | 3014 |
| `www.offensivetools.thm` | 200 ✅ | 8806 |
| `secret.offensivetools.thm` | 200 ✅ | 1550 |
| `WWW.offensivetools.thm` | 200 ✅ | 8806 |

All of these are running on the **same IP** (`10.113.166.64`) — just different virtual host configurations on the web server.

---

## 📊 HTTP Status Codes Cheatsheet

| Code | Meaning | Gobuster Context |
|------|---------|-----------------|
| **200** | OK — Page exists & accessible | High priority target |
| **301** | Moved Permanently — Redirect | Path exists, follow it |
| **403** | Forbidden — Exists but blocked | Interesting — try harder |
| **404** | Not Found | Doesn't exist, skip |

---

## 🧠 Concepts Learned

| Concept | Explanation |
|---------|-------------|
| ✅ **Enumeration** | Systematically listing things — directories, files, subdomains |
| ✅ **Brute Force** | Trying every possibility from a wordlist |
| ✅ **Wordlists** | Files with thousands of common names to try |
| ✅ **dir mode** | Finding hidden pages/folders on a website |
| ✅ **dns mode** | Discovering subdomains via DNS queries |
| ✅ **vhost mode** | Finding virtual hosts sharing the same IP |
| ✅ **Extension fuzzing** | Adding `-x` to find specific file types |
| ✅ **curl** | Fetching a URL's content directly from terminal |
| ✅ **False positive filtering** | Using `--exclude-length` to remove noise |

---

> 📝 *Notes by [Muhammad Ali](https://tryhackme.com/p/Muhammad.Ali12) | TryHackMe: Muhammad.Ali12*
