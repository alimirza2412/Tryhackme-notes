# 🐧 TryHackMe — Linux Fundamentals Part 2

> **Room:** Linux Fundamentals Part 2
> **Platform:** TryHackMe
> **Difficulty:** Easy 🟢
> **Author of notes:** Muhammad Ali (`Muhammad.Ali12`)
> **Goal:** Learn to log in with SSH, use flags, manage files, understand permissions and important folders.

---

## 📑 Table of Contents

- [1. 🎯 Room Overview](#1--room-overview)
- [2. 🔐 Logging in with SSH](#2--logging-in-with-ssh)
- [3. 🚩 Flags and Switches](#3--flags-and-switches)
- [4. 📂 File Commands (Continued)](#4--file-commands-continued)
- [5. 🛂 Permissions 101](#5--permissions-101)
- [6. 👑 Users, su and sudo](#6--users-su-and-sudo)
- [7. 🗂️ Common Directories](#7-️-common-directories)
- [8. ✅ Task Answers](#8--task-answers)
- [9. 🧾 Cheat Sheet](#9--cheat-sheet)
- [10. 💡 Key Takeaways](#10--key-takeaways)

---

## 1. 🎯 Room Overview

| Item | Detail |
|------|--------|
| 🧪 Room Name | Linux Fundamentals Part 2 |
| 📚 Topics | SSH, flags, files, permissions, common folders |
| 🖥️ Machine | Target machine (connect using SSH) |
| ⬅️ Previous | Linux Fundamentals Part 1 |
| ➡️ Next | Linux Fundamentals Part 3 |

📸 **Screenshot:** _Room completion badge_
`![Room Complete](screenshots/part2-room-complete.png)`

---

## 2. 🔐 Logging in with SSH

**SSH** = **S**ecure **Sh**ell.

**Analogy:** SSH is like a **secret tunnel** 🚇 from your computer to another computer. Everything inside the tunnel is locked, so nobody can spy on it.

```
 💻 Your PC  ══════ 🔒 encrypted tunnel ══════►  🖥️ Remote Linux Machine
   (client)                                          (server)
```

### Command

```bash
ssh username@MACHINE_IP
```

| Part | Meaning |
|------|---------|
| `ssh` | The tool |
| `username` | Who you log in as |
| `@` | "at" |
| `MACHINE_IP` | Address of the target machine |

### Steps

1. Start the target machine in TryHackMe and copy its IP.
2. Open your terminal (or AttackBox).
3. Run `ssh username@MACHINE_IP`.
4. Type `yes` if it asks about the fingerprint.
5. Enter the password. Nothing shows while you type. That is normal. 🙈

📸 **Screenshot:** _SSH login success_
`![SSH Login](screenshots/ssh-login.png)`

---

## 3. 🚩 Flags and Switches

**Analogy:** A command is like a **coffee order** ☕. Flags are the extras: "extra sugar", "no milk".

Flags start with `-` (short) or `--` (long).

| Command | What the flag does |
|---------|--------------------|
| `ls` | Shows normal files |
| `ls -a` | Shows **all** files, including hidden ones (start with `.`) |
| `ls -l` | Shows a **long** list with details (owner, size, permissions) |
| `ls -la` | Both together |
| `ls --help` | Shows help for that command |

### Need help? Use `man`

```bash
man ls        # opens the manual for ls
# press q to quit
```

```
 command  +  flag   =  new behaviour
   ls        -la       show ALL files with DETAILS
```

📸 **Screenshot:** _ls -la output_
`![ls -la](screenshots/ls-la.png)`

---

## 4. 📂 File Commands (Continued)

| Command | Meaning | Example |
|---------|---------|---------|
| `touch` | Create an empty file | `touch note` |
| `mkdir` | Create a folder | `mkdir mydir` |
| `cp` | Copy a file | `cp note note2` |
| `mv` | Move or rename | `mv note newname` |
| `rm` | Delete a file | `rm note` |
| `rm -r` | Delete a folder | `rm -r mydir` |
| `file` | Tell the file type | `file note` |

> ⚠️ **Remember:** `rm` has no undo. Always double-check.

> 💡 **Linux does not care about extensions.** A file called `pic.txt` can still be an image. Use `file` to find the real type.

---

## 5. 🛂 Permissions 101

**Analogy:** Permissions are like **keys to rooms** 🔑 in a building. Some people can enter and change things, some can only look.

### Reading `ls -l`

```
 -rwxr-xr--  1  ali  staff  120  Jan 1  script.sh
 │└┬┘└┬┘└┬┘
 │ │  │  └── others  (r--)  read only
 │ │  └───── group   (r-x)  read + execute
 │ └──────── owner   (rwx)  read + write + execute
 └────────── type: - = file, d = directory
```

### The three permissions

| Letter | Name | On a file | On a folder |
|--------|------|-----------|-------------|
| `r` | Read | View content | List what is inside |
| `w` | Write | Change content | Add or delete files |
| `x` | Execute | Run as program | Enter the folder (`cd`) |

### The three groups of people

| Who | Meaning |
|-----|---------|
| 👤 Owner (user) | The person who owns the file |
| 👥 Group | A team of users |
| 🌍 Others | Everyone else |

---

## 6. 👑 Users, su and sudo

| Command | Meaning | Analogy |
|---------|---------|---------|
| `su username` | Switch to another user | Change into someone else's uniform 🎭 |
| `sudo command` | Run one command as admin (root) | Borrow the boss's key for one door 🗝️ |
| `whoami` | Show the current user | "Who am I?" |

```bash
su root              # switch to root (needs root password)
sudo cat /etc/shadow # run one command as root
```

### Important user files

| File | What it stores |
|------|----------------|
| `/etc/passwd` | List of users on the system |
| `/etc/shadow` | Password **hashes** (only root can read) |
| `/etc/sudoers` | Who is allowed to use `sudo` |

> 🛡️ **Security note:** If a normal user can read `/etc/shadow`, that is a big misconfiguration. An attacker can try to crack the hashes.

📸 **Screenshot:** _su and sudo usage_
`![su sudo](screenshots/su-sudo.png)`

---

## 7. 🗂️ Common Directories

```
/
├── etc   ⚙️  config files (passwd, shadow, sudoers)
├── var   📝  logs and changing data (/var/log, /var/www)
├── root  👑  home folder of the root user
└── tmp   🗑️  temporary files, anyone can write
```

| Folder | What is inside | Why hackers care |
|--------|----------------|------------------|
| `/etc` | System settings | Passwords, users, sudo rules |
| `/var` | Logs, web files, mail | See what happened, find web apps |
| `/root` | Root's personal files | Only root can enter, big target |
| `/tmp` | Short-lived files | World-writable, good for dropping files |

**Analogy for `/tmp`:** a **public notice board** 📌. Anyone can stick something on it, and it gets cleaned up often.

---

## 8. ✅ Task Answers

> Fill in your own answers after solving the room.

| Task | Topic | Answer |
|------|-------|--------|
| 1 | Introduction | No answer needed |
| 2 | Accessing your Linux machine using SSH | _your answer_ |
| 3 | Introduction to flags and switches | _your answer_ |
| 4 | Filesystem interaction continued | _your answer_ |
| 5 | Permissions 101 | _your answer_ |
| 6 | Common directories | _your answer_ |

---

## 9. 🧾 Cheat Sheet

| Goal | Command |
|------|---------|
| Log in to remote machine | `ssh user@IP` |
| Show hidden files | `ls -a` |
| Show details | `ls -l` |
| Read the manual | `man <command>` |
| Quick help | `<command> --help` |
| Create file / folder | `touch` / `mkdir` |
| Copy / move / delete | `cp` / `mv` / `rm` |
| Find file type | `file` |
| Switch user | `su <user>` |
| Run as admin | `sudo <command>` |
| See users | `cat /etc/passwd` |

---

## 10. 💡 Key Takeaways

- ✅ SSH is a safe tunnel to control another machine from your terminal.
- ✅ Flags change how a command works. `ls -la` is one you will use daily.
- ✅ Hidden files start with a dot. Always check with `-a`.
- ✅ Permissions are `rwx` for owner, group, and others.
- ✅ `sudo` gives temporary admin power. `su` switches the whole user.
- ✅ `/etc` and `/var` are the first places to look in CTFs and pentests.

---

## 🔗 Links

- 🧪 Room: [TryHackMe — Linux Fundamentals Part 2](https://tryhackme.com/room/linuxfundamentalspart2)
- ⬅️ Previous: Linux Fundamentals Part 1
- ➡️ Next: Linux Fundamentals Part 3

---

⭐ _Notes written while learning on TryHackMe. Practice every command yourself in the room._
