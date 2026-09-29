# 🐧 TryHackMe — Linux Fundamentals Part 3

> **Room:** Linux Fundamentals Part 3
> **Platform:** TryHackMe
> **Difficulty:** Easy 🟢
> **Author of notes:** Muhammad Ali (`Muhammad.Ali12`)
> **Goal:** Learn text editors, download and transfer files, manage processes, automate tasks, install software, and read logs.

---

## 📑 Table of Contents

- [1. 🎯 Room Overview](#1--room-overview)
- [2. ✍️ Terminal Text Editors](#2-️-terminal-text-editors)
- [3. 🛠️ Useful Utilities](#3-️-useful-utilities)
- [4. ⚙️ Processes](#4-️-processes)
- [5. 🔄 Foreground and Background](#5--foreground-and-background)
- [6. ⏰ Automation with Cron](#6--automation-with-cron)
- [7. 📦 Package Management](#7--package-management)
- [8. 📝 Logs](#8--logs)
- [9. ✅ Task Answers](#9--task-answers)
- [10. 🧾 Cheat Sheet](#10--cheat-sheet)
- [11. 💡 Key Takeaways](#11--key-takeaways)

---

## 1. 🎯 Room Overview

| Item | Detail |
|------|--------|
| 🧪 Room Name | Linux Fundamentals Part 3 |
| 📚 Topics | Editors, wget/scp, processes, cron, apt, logs |
| ⬅️ Previous | Linux Fundamentals Part 2 |
| 🏁 Series | Last room of Linux Fundamentals |

📸 **Screenshot:** _Room completion badge_
`![Room Complete](screenshots/part3-room-complete.png)`

---

## 2. ✍️ Terminal Text Editors

**Analogy:** Notepad, but you use only the keyboard. No mouse. ⌨️

### 2.1 Nano (easy)

```bash
nano myfile.txt
```

| Shortcut | Action |
|----------|--------|
| `Ctrl + O` | Save (write out) |
| `Ctrl + X` | Exit |
| `Ctrl + W` | Search |
| `Ctrl + K` | Cut a line |
| `Ctrl + U` | Paste |
| `Ctrl + G` | Help |

### 2.2 Vim (powerful)

Vim has **modes**. Think of it like a TV remote with different button sets. 📺

```
   ┌──────────────┐   press i    ┌──────────────┐
   │ Normal Mode  │ ───────────► │ Insert Mode  │
   │ (commands)   │ ◄─────────── │ (typing)     │
   └──────┬───────┘   press Esc  └──────────────┘
          │ press :
          ▼
   ┌──────────────┐
   │ Command Mode │  :w save  :q quit  :wq save+quit  :q! quit no save
   └──────────────┘
```

| Editor | Good for | Learning speed |
|--------|----------|----------------|
| Nano | Quick edits, beginners | 🟢 Fast |
| Vim | Big edits, power users | 🔴 Slower |

📸 **Screenshot:** _Editing a file with nano_
`![Nano](screenshots/nano.png)`

---

## 3. 🛠️ Useful Utilities

### 3.1 `wget` — Download a file

```bash
wget https://example.com/file.txt
```

### 3.2 `scp` — Copy files over SSH

**Analogy:** Sending a parcel 📦 through the secure SSH tunnel.

```bash
# Local  ➜ Remote
scp file.txt user@IP:/home/user/

# Remote ➜ Local
scp user@IP:/home/user/file.txt .
```

### 3.3 Quick web server with Python

Share files from your machine in one line.

```bash
python3 -m http.server 8000
# Others can now download from http://YOUR_IP:8000
```

| Tool | Job | Direction |
|------|-----|-----------|
| `wget` | Download from a URL | Internet ➜ You |
| `scp` | Copy over SSH | You ⇄ Remote machine |
| `http.server` | Serve your files | You ➜ Others |

📸 **Screenshot:** _wget or python server in use_
`![Utilities](screenshots/utilities.png)`

---

## 4. ⚙️ Processes

A **process** is a program that is running. Every process gets a number called **PID** (Process ID).

**Analogy:** Processes are like **workers in a factory** 🏭. Each worker has an ID badge (PID).

### Viewing processes

| Command | What it shows |
|---------|---------------|
| `ps` | Your own processes |
| `ps aux` | All processes, all users |
| `top` | Live list, updates every few seconds (`q` to quit) |

### Killing processes

```bash
kill 1234        # politely ask PID 1234 to stop
kill -9 1234     # force stop, no questions
```

| Signal | Meaning | Analogy |
|--------|---------|---------|
| `SIGTERM` | "Please stop" (default) | Asking nicely 🙂 |
| `SIGKILL` | "Stop now" (`-9`) | Pulling the plug 🔌 |
| `SIGSTOP` | "Pause" | Pressing pause ⏸️ |

> 💡 **PID 1** is the first process that starts when Linux boots (`systemd`). Every other process comes from it.

### Managing services with `systemctl`

A **service** is a program that runs in the background (like a web server).

```bash
systemctl start apache2      # start now
systemctl stop apache2       # stop now
systemctl enable apache2     # start automatically at boot
systemctl disable apache2    # do not start at boot
systemctl status apache2     # check if running
```

| Command | Now | After reboot |
|---------|-----|--------------|
| `start` | ✅ On | No change |
| `stop` | ❌ Off | No change |
| `enable` | No change | ✅ On |
| `disable` | No change | ❌ Off |

📸 **Screenshot:** _ps aux or top output_
`![Processes](screenshots/processes.png)`

---

## 5. 🔄 Foreground and Background

| Action | How |
|--------|-----|
| Run in background | Add `&` → `command &` |
| Pause a running command | `Ctrl + Z` |
| Bring it back to front | `fg` |

```
 Foreground 🖥️            Background 🌙
 (you wait for it)        (runs quietly, you keep working)
```

---

## 6. ⏰ Automation with Cron

**Cron** runs tasks automatically on a schedule. **Analogy:** an alarm clock ⏰ that runs a command instead of ringing.

```bash
crontab -e      # edit your schedule
crontab -l      # list your schedule
```

### The 5 time fields

```
 ┌───────────── minute        (0 - 59)
 │ ┌─────────── hour          (0 - 23)
 │ │ ┌───────── day of month  (1 - 31)
 │ │ │ ┌─────── month         (1 - 12)
 │ │ │ │ ┌───── day of week   (0 - 6, Sunday = 0)
 │ │ │ │ │
 * * * * *   command to run
```

### Examples

| Schedule | Meaning |
|----------|---------|
| `* * * * *` | Every minute |
| `0 * * * *` | Every hour |
| `30 2 * * *` | Every day at 2:30 AM |
| `0 0 * * 0` | Every Sunday at midnight |
| `*/5 * * * *` | Every 5 minutes |

> `*` means "every". `*/5` means "every 5".

> 🛡️ **Security note:** Cron jobs running as root can be a way for attackers to get higher access if the script is editable by anyone.

---

## 7. 📦 Package Management

**Analogy:** `apt` is like an **app store** 🏪 for Linux, in the terminal.

| Command | What it does |
|---------|--------------|
| `sudo apt update` | Refresh the list of available software |
| `sudo apt upgrade` | Update installed software |
| `sudo apt install nmap` | Install a package |
| `sudo apt remove nmap` | Remove a package |
| `sudo add-apt-repository <repo>` | Add an extra software source |

```
  📚 Repository (software list)
         │  apt update  (get the list)
         ▼
  🖥️ Your machine ──► apt install ──► ✅ Program ready
```

> ⚠️ Only add repositories you trust. A bad repository can install harmful software.

---

## 8. 📝 Logs

Logs are the **diary** 📖 of the system. They record what happened and when.

They live in `/var/log`.

| Log / Folder | What it records |
|--------------|-----------------|
| `/var/log/apache2/access.log` | Who visited the web server |
| `/var/log/apache2/error.log` | Web server errors |
| `/var/log/auth.log` | Logins and `sudo` use |
| `/var/log/syslog` | General system messages |

```bash
cat /var/log/apache2/access.log
tail -f /var/log/auth.log      # watch new lines live
grep "Failed" /var/log/auth.log
```

> 🕵️ **Why it matters:** Defenders read logs to catch attacks. Attackers often try to hide their steps in logs.

📸 **Screenshot:** _Reading a log file_
`![Logs](screenshots/logs.png)`

---

## 9. ✅ Task Answers

> Fill in your own answers after solving the room.

| Task | Topic | Answer |
|------|-------|--------|
| 1 | Introduction | No answer needed |
| 2 | Terminal text editors | _your answer_ |
| 3 | General/useful utilities | _your answer_ |
| 4 | Processes 101 | _your answer_ |
| 5 | Maintaining your system: automation | _your answer_ |
| 6 | Maintaining your system: package management | _your answer_ |
| 7 | Maintaining your system: logs | _your answer_ |

---

## 10. 🧾 Cheat Sheet

| Goal | Command |
|------|---------|
| Edit a file (easy) | `nano file` |
| Edit a file (advanced) | `vim file` |
| Download a file | `wget URL` |
| Copy over SSH | `scp src user@IP:dest` |
| Quick web server | `python3 -m http.server` |
| List processes | `ps aux` |
| Live processes | `top` |
| Stop a process | `kill PID` / `kill -9 PID` |
| Manage a service | `systemctl start/stop/enable/disable` |
| Run in background | `command &` |
| Bring back to front | `fg` |
| Edit schedule | `crontab -e` |
| Install software | `sudo apt install <name>` |
| Update package list | `sudo apt update` |
| Read logs | `cat` / `tail -f` in `/var/log` |

---

## 11. 💡 Key Takeaways

- ✅ Nano is easy. Vim is powerful but needs practice.
- ✅ `wget` downloads, `scp` transfers over SSH, `http.server` shares.
- ✅ Every process has a PID. `kill -9` is the last option.
- ✅ `systemctl enable` decides what starts at boot.
- ✅ Cron = scheduled tasks. Five fields: minute, hour, day, month, weekday.
- ✅ `apt` installs software. Trust your sources.
- ✅ Logs in `/var/log` tell the story of the system.

---

## 🔗 Links

- 🧪 Room: [TryHackMe — Linux Fundamentals Part 3](https://tryhackme.com/room/linuxfundamentalspart3)
- ⬅️ Previous: Linux Fundamentals Part 2

---

⭐ _Notes written while learning on TryHackMe. Practice every command yourself in the room._
