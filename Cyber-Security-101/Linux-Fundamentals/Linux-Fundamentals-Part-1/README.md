# 🐧 TryHackMe — Linux Fundamentals Part 1

> **Room:** Linux Fundamentals Part 1
> **Platform:** TryHackMe
> **Difficulty:** Easy 🟢
> **Author of notes:** Muhammad Ali (`Muhammad.Ali12`)
> **Goal:** Learn the basic Linux commands, and how to move around and work in the terminal.

---

## 📑 Table of Contents

- [1. 🎯 Room Overview](#1--room-overview)
- [2. 🧠 What is Linux? (Analogy)](#2--what-is-linux-analogy)
- [3. 🌍 Where is Linux Used?](#3--where-is-linux-used)
- [4. 🎨 Linux Flavours (Distros)](#4--linux-flavours-distros)
- [5. 💻 Your First Commands](#5--your-first-commands)
- [6. 📂 Working with Files and Folders](#6--working-with-files-and-folders)
- [7. 🔍 Searching for Files](#7--searching-for-files)
- [8. 🧩 Shell Operators](#8--shell-operators)
- [9. 🗺️ Linux Folder Map](#9-️-linux-folder-map)
- [10. ✅ Task Answers](#10--task-answers)
- [11. 🧾 Cheat Sheet](#11--cheat-sheet)
- [12. 💡 Key Takeaways](#12--key-takeaways)

---

## 1. 🎯 Room Overview

| Item | Detail |
|------|--------|
| 🧪 Room Name | Linux Fundamentals Part 1 |
| 📚 Topics | Linux intro, basic commands, files, search, operators |
| 🖥️ Machine | Browser-based Linux machine (AttackBox / Target Machine) |
| 🔗 Next Room | Linux Fundamentals Part 2 |

📸 **Screenshot:** _Room completion badge_
`![Room Complete](screenshots/room-complete.png)`

---

## 2. 🧠 What is Linux? (Analogy)

Think of an operating system (OS) like **a restaurant manager**.

- 🧑‍🍳 Hardware (CPU, RAM, disk) = the kitchen and workers
- 🙋 You (the user) = the customer
- 🧑‍💼 OS (Linux) = the manager who takes your order and tells the kitchen what to do

Linux is a **free and open-source** OS. Anyone can see the code, change it, and share it.

```
   👤 You
     │  (type a command)
     ▼
 ┌───────────┐
 │   Shell   │  ← Takes your command
 └─────┬─────┘
       ▼
 ┌───────────┐
 │  Kernel   │  ← The real boss, talks to hardware
 └─────┬─────┘
       ▼
 ┌───────────┐
 │ Hardware  │  ← CPU, RAM, Disk
 └───────────┘
```

---

## 3. 🌍 Where is Linux Used?

| Place | Example |
|-------|---------|
| 🌐 Web servers | Most websites run on Linux |
| 📱 Phones | Android is built on Linux |
| 🏢 Companies | Servers, cloud, databases |
| 🚗 Devices | Smart TVs, routers, cars |
| 🛡️ Cybersecurity | Kali Linux, Parrot OS |

> 💡 **Why hackers love Linux:** Most servers you will test run Linux, and most security tools are made for it.

---

## 4. 🎨 Linux Flavours (Distros)

A **distro** (distribution) is like a **car brand**. Same engine idea (Linux kernel), different looks and features.

| Distro | Best For |
|--------|----------|
| 🟠 Ubuntu | Beginners, daily use, servers |
| 🔵 Debian | Stable servers |
| 🟣 Kali Linux | Penetration testing |
| 🔴 Red Hat / CentOS | Company servers |
| 🟢 Linux Mint | Easy desktop use |

The room uses **Ubuntu**.

---

## 5. 💻 Your First Commands

### `echo` — Say something
Prints text on the screen (like a parrot 🦜).

```bash
echo Hello TryHackMe
# Output: Hello TryHackMe
```

### `whoami` — Who am I?
Shows the current username.

```bash
whoami
# Output: tryhackme
```

📸 **Screenshot:** _echo and whoami output_
`![Echo Whoami](screenshots/echo-whoami.png)`

---

## 6. 📂 Working with Files and Folders

### 6.1 Moving Around

| Command | Meaning | Analogy |
|---------|---------|---------|
| `ls` | List files in current folder | Look around the room 👀 |
| `cd <folder>` | Change directory | Walk into another room 🚪 |
| `pwd` | Print working directory | "Where am I?" on a map 📍 |

```bash
ls               # see what is here
cd Documents     # go into Documents
pwd              # shows /home/tryhackme/Documents
cd ..            # go one step back
```

### 6.2 Reading Files

| Command | What it does |
|---------|--------------|
| `cat file.txt` | Show the whole file |
| `touch file.txt` | Create an empty file |
| `mkdir folder` | Make a new folder |

```bash
cat note.txt
touch newfile.txt
mkdir myfolder
```

### 6.3 Copy, Move, Delete

| Command | Meaning | Example |
|---------|---------|---------|
| `cp` | Copy | `cp a.txt b.txt` |
| `mv` | Move or rename | `mv a.txt /tmp/` |
| `rm` | Delete file | `rm a.txt` |
| `rm -r` | Delete folder | `rm -r myfolder` |
| `file` | Show file type | `file a.txt` |

> ⚠️ **Warning:** `rm` has **no recycle bin**. Deleted means gone.

📸 **Screenshot:** _File commands in the terminal_
`![File Commands](screenshots/file-commands.png)`

---

## 7. 🔍 Searching for Files

### `find` — Search by name
```bash
find / -name passwords.txt
find /home -name "*.txt"       # * means "anything"
```

### `grep` — Search inside a file
Like pressing **Ctrl + F** in a document.

```bash
grep "admin" access.log
```

| Tool | Searches | Analogy |
|------|----------|---------|
| `find` | File **names** | Looking for a book on the shelf 📚 |
| `grep` | File **content** | Reading pages to find one word 🔎 |

---

## 8. 🧩 Shell Operators

Operators connect commands together, like LEGO blocks 🧱.

| Operator | Name | What it does | Example |
|----------|------|--------------|---------|
| `&` | Background | Run command in the background | `sleep 100 &` |
| `&&` | AND | Run 2nd command only if 1st works | `cd docs && ls` |
| `>` | Redirect | Save output to a file (overwrites) | `echo hi > a.txt` |
| `>>` | Append | Add output to end of file | `echo bye >> a.txt` |

```
 echo hello  ──►  >  ──►  file.txt
   (output)     (arrow)   (saved here)
```

📸 **Screenshot:** _Operators in action_
`![Operators](screenshots/operators.png)`

---

## 9. 🗺️ Linux Folder Map

```
/
├── bin      → basic programs (ls, cat)
├── etc      → system settings (config files) ⚙️
├── home     → user folders 🏠
├── root     → the admin's home folder 👑
├── tmp      → temporary files (cleared often) 🗑️
├── usr      → user programs and apps
└── var      → logs and changing data 📝
```

| Folder | Why it matters for hackers |
|--------|----------------------------|
| `/etc` | Holds config files, passwords (`/etc/passwd`, `/etc/shadow`) |
| `/var/log` | Logs show what happened on the system |
| `/tmp` | Anyone can write here |
| `/root` | The most powerful user's folder |

---

## 10. ✅ Task Answers

> Add your own answers here after solving the room.

| Task | Question | Answer |
|------|----------|--------|
| 1 | Introduction | No answer needed |
| 2 | A bit of background on Linux | _your answer_ |
| 3 | Running your first few commands | _your answer_ |
| 4 | Interacting with the filesystem | _your answer_ |
| 5 | Searching for files | _your answer_ |
| 6 | An introduction to shell operators | _your answer_ |

---

## 11. 🧾 Cheat Sheet

| Goal | Command |
|------|---------|
| Print text | `echo` |
| Current user | `whoami` |
| List files | `ls` |
| Change folder | `cd` |
| Current location | `pwd` |
| Read file | `cat` |
| Create file | `touch` |
| Create folder | `mkdir` |
| Copy | `cp` |
| Move or rename | `mv` |
| Delete | `rm` |
| File type | `file` |
| Find by name | `find` |
| Find by content | `grep` |
| Run in background | `&` |
| Chain commands | `&&` |
| Save output | `>` / `>>` |

---

## 12. 💡 Key Takeaways

- ✅ Linux is free, open-source, and used almost everywhere.
- ✅ The terminal is faster and more powerful than clicking.
- ✅ `ls`, `cd`, `pwd`, `cat` are the four commands you will use every day.
- ✅ `find` searches names, `grep` searches content.
- ✅ Be careful with `rm`. There is no undo.
- ✅ Knowing the folder map helps a lot when hunting for files in CTFs.

---

## 🔗 Links

- 🧪 Room: [TryHackMe — Linux Fundamentals Part 1](https://tryhackme.com/room/linuxfundamentalspart1)
- 📘 Next: Linux Fundamentals Part 2

---

⭐ _Notes written while learning on TryHackMe. Practice every command yourself in the room._
