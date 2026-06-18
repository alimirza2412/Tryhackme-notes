# 🛠️ Offensive Security Tooling

> **This section of the Cyber Security 101 path covers the core tools used in real-world penetration testing.**  
> From enumeration to brute-forcing, from shells to SQL injection — each tool has its role in an attacker's arsenal. 💀

---

## 📁 Rooms Covered

| # | Room | What You Learn | Status |
|---|------|---------------|--------|
| 1 | 🔍 [Gobuster: The Basics](./Gobuster:%20The%20Basics/) | Directory & DNS enumeration, wordlists, HTTP status codes | ✅ Done |
| 2 | 🔐 [Hydra](./Hydra/) | SSH & HTTP brute-forcing, rockyou.txt, login attacks | ✅ Done |
| 3 | 🐚 [Shell Overview](./Shell-Overview/) | Reverse shells, bind shells, web shells, netcat listeners | ✅ Done |
| 4 | 💉 [SQLMap: The Basics](./SQLMap:The-Basics/) | SQL injection automation, database enumeration, data dumping | ✅ Done |

---

## 🧠 Tools at a Glance

| Tool | What It Does | Real-World Use |
|------|-------------|----------------|
| **Gobuster** | Discovers hidden files, directories, and subdomains | Web server enumeration during recon phase |
| **Hydra** | Brute-forces credentials across multiple protocols | Attacking login pages, SSH, FTP services |
| **Netcat / Shells** | Establishes remote command execution (reverse or bind) | Gaining and maintaining access to a target |
| **SQLMap** | Automatically detects and exploits SQL injection flaws | Extracting data from vulnerable databases |

---

## 🎯 What is Offensive Security Tooling?

> Think of it this way: if **network scanning** tools (Nmap, Wireshark) answer *"what's running?"* —  
> then **offensive tools** answer *"how do we get in?"* 🎭

These tools cover the core phases of a penetration test:

- 🔍 **Enumeration** → mapping the attack surface (Gobuster)
- 🔓 **Credential Attacks** → cracking weak or default passwords (Hydra)
- 🐚 **Post-Exploitation** → maintaining access after a foothold (Shells)
- 💉 **Injection Attacks** → exploiting vulnerable databases (SQLMap)

---

## ⚠️ Disclaimer

> These notes are strictly for **educational purposes**.  
> Only use these tools on **systems you own or have explicit permission to test** (e.g. TryHackMe labs).  
> Unauthorized use is illegal. 🚫

---

## 🔗 Related Sections

- [← Cyber Security 101](../README.md)
- [→ Jr Penetration Tester Path](../../Jr-Penetration-Tester/README.md)

---

*Made with 💻 by [alimirza2412](https://github.com/alimirza2412) | TryHackMe: [Muhammad.Ali12](https://tryhackme.com/p/Muhammad.Ali12)*
