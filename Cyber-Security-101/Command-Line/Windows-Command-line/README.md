# 🖥️ Windows Command Line — TryHackMe Room Notes

> **Room:** Windows Command Line  
> **Path:** Cyber Security 101
> **Difficulty:** Easy  
> **Status:** ✅ Completed

---

## 📋 Table of Contents

- [Why CLI over GUI?](#-why-cli-over-gui)
- [System Information](#-system-information)
- [Network Configuration](#-network-configuration)
- [Network Troubleshooting](#-network-troubleshooting)
- [Working With Directories](#-working-with-directories)
- [Working With Files](#-working-with-files)
- [Process Management](#-process-management)
- [General Utility Commands](#-general-utility-commands)
- [Key Flags Reference](#-key-flags-reference)
- [⚡ Quick Cheatsheet](#-quick-cheatsheet)

---

## 💡 Why CLI over GUI?

| Feature | GUI | CLI |
|---|---|---|
| Ease of use | ✅ Intuitive | ❌ Learning curve |
| Speed | ❌ Multiple clicks | ✅ One command |
| Automation | ❌ Hard to script | ✅ Batch/script ready |
| Remote Management | ❌ Needs display | ✅ Works over SSH |
| Resource Usage | ❌ Graphics-heavy | ✅ Lightweight |

---

## 🖥️ System Information

```cmd
ver                  # Windows version
systeminfo           # Full system info — OS, RAM, CPU, hotfixes
set                  # Environment variables (includes PATH)
driverquery          # Installed device drivers
sfc /scannow         # Scan and repair system files
chkdsk               # Check disk for errors and bad sectors
```

> **Tip:** Long output? Pipe it through `more`:
> ```cmd
> systeminfo | more
> driverquery | more
> ```
> Spacebar = next page | `Ctrl+C` = exit

### PATH — Why It Matters

```
Path=C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;...
```

Windows only runs commands found in these directories. If you get `command not recognized`, it's not in PATH.

---

## 🌐 Network Configuration

```cmd
ipconfig             # IP address, subnet mask, default gateway
ipconfig /all        # Full detail — MAC, DHCP, DNS, lease time
```

### `ipconfig /all` Output Breakdown

| Field | Meaning |
|---|---|
| IPv4 Address | Your machine's IP |
| Subnet Mask | Network size |
| Default Gateway | Router IP — exit point to internet |
| Physical Address | MAC address (hardware ID) |
| DHCP Enabled | IP assigned automatically? |
| DNS Servers | Domain name resolver |
| Lease Obtained / Expires | How long DHCP gave you this IP |

---

## 🔍 Network Troubleshooting

### ping — Test Reachability

```cmd
ping example.com
```

Sends 4 ICMP packets. If replies come back — target is reachable and can reach you.

```
Reply from 93.184.215.14: bytes=32 time=78ms TTL=52
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
Average = 78ms
```

| Result | Meaning |
|---|---|
| 0% loss | Stable connection |
| High RTT (ms) | Slow/congested network |
| Request timed out | Host unreachable or blocking ICMP |

---

### tracert — Trace Route

```cmd
tracert example.com
```

Shows every router (hop) between you and the target. Each line = one hop with 3 time readings.

```
ASCII Diagram — tracert flow:

[Your Machine]
     |
     v
[Hop 1 — Local Router]        59ms
     |
     v
[Hop 2 — * * *]               (ICMP blocked — normal)
     |
     v
[Hop 9 — ISP Router]          <1ms
     |
     v
[Hop 15 — Edge CDN]           81ms
     |
     v
[Target: 93.184.215.14]       78ms
```

> `* * *` = that router didn't reply (firewall). Not always an error.

---

### nslookup — DNS Lookup

```cmd
nslookup example.com            # Use default DNS server
nslookup example.com 1.1.1.1    # Use Cloudflare DNS (1.1.1.1)
```

Resolves domain → IP. Useful to check if DNS is working correctly, or compare responses from different DNS servers.

---

### netstat — Active Connections

```cmd
netstat              # Only ESTABLISHED connections
netstat -abon        # Full detail — all ports, PIDs, process names
```

#### netstat Flags

| Flag | What It Shows |
|---|---|
| `-a` | All connections + listening ports |
| `-b` | Executable associated with each port |
| `-o` | Process ID (PID) |
| `-n` | Numerical form (no DNS resolve — faster) |

#### Connection States

| State | Meaning |
|---|---|
| `LISTENING` | Port is open, waiting for connections |
| `ESTABLISHED` | Active connection currently running |

```
Example output of netstat -abon:

Proto  Local Address     Foreign Address   State       PID
TCP    0.0.0.0:22        0.0.0.0:0         LISTENING   2116
[sshd.exe]
TCP    10.10.230.237:22  10.11.81.126:xxx  ESTABLISHED 2116
[sshd.exe]
```

> **Port 22 = SSH** — if you see it ESTABLISHED, an SSH session is active.

---

## 📁 Working With Directories

```cmd
cd                   # Show current directory (where am I?)
cd [folder]          # Navigate into a folder
cd ..                # Go one level up
dir                  # List files and folders
dir /a               # Include hidden and system files
dir /s               # Include all subdirectories recursively
tree                 # Visual folder structure
mkdir [name]         # Create new directory
rmdir [name]         # Delete directory (must be empty)
```

### Navigation Example

```
C:\>cd Users
C:\Users>cd strategos
C:\Users\strategos>cd ..
C:\Users>cd ..
C:\>
```

### tree Output Example

```
C:.
├───Desktop
├───Documents
├───Downloads
├───Music
└───Videos
```

---

## 📄 Working With Files

```cmd
type file.txt               # Print file contents to terminal
more file.txt               # View large files page by page
copy source.txt dest.txt    # Copy a file
copy *.md C:\Markdown       # Copy all .md files (wildcard)
move file.txt ..            # Move file one level up
del file.txt                # Delete file
erase file.txt              # Same as del
```

### Wildcard `*` — Batch Operations

```cmd
copy *.txt C:\backup        # All .txt files
del *.log                   # Delete all .log files
move *.md C:\Notes          # Move all markdown files
```

---

## ⚙️ Process Management

```cmd
tasklist                                      # All running processes + PID + memory
tasklist /FI "imagename eq sshd.exe"          # Filter by process name
taskkill /PID 4567                            # Kill process by PID
```

### tasklist Output

```
Image Name          PID   Session Name   Mem Usage
============        ===   ============   =========
sshd.exe           2116   Services        6,992 K
sshd.exe           2712   Services        7,668 K
```

### Pentesting Workflow — Find and Kill Suspicious Process

```
Step 1: netstat -abon
        └─> Find suspicious port + its PID

Step 2: tasklist
        └─> Match PID to process name

Step 3: taskkill /PID [number]
        └─> Terminate the process
```

---

## 🛠️ General Utility Commands

```cmd
help [command]       # Detailed help for any command
command /?           # Same as help — short form
cls                  # Clear the terminal screen
shutdown /s          # Shut down the system
shutdown /r          # Restart the system
shutdown /a          # Abort a scheduled shutdown
```

---

## 📌 Key Flags Reference

### netstat

| Command | Use Case |
|---|---|
| `netstat` | Quick look at active connections |
| `netstat -a` | See all listening ports |
| `netstat -abon` | Full detail for security analysis |

### dir

| Command | Use Case |
|---|---|
| `dir` | Basic listing |
| `dir /a` | Reveal hidden files |
| `dir /s` | Recursive — all subfolders |

### shutdown

| Command | Action |
|---|---|
| `shutdown /s` | Shutdown |
| `shutdown /r` | Restart |
| `shutdown /a` | Abort scheduled shutdown |

---

## ⚡ Quick Cheatsheet

> Copy-paste ready — saare important commands ek jagah

### 🖥️ System Info

| Command | What It Does |
|---|---|
| `ver` | Windows version |
| `systeminfo` | Full OS, RAM, CPU info |
| `set` | Environment variables + PATH |
| `driverquery` | Installed drivers list |
| `sfc /scannow` | Scan & repair system files |
| `chkdsk` | Check disk for errors |
| `cls` | Clear screen |
| `help [cmd]` / `cmd /?` | Get help for any command |
| `shutdown /s` | Shutdown |
| `shutdown /r` | Restart |
| `shutdown /a` | Abort scheduled shutdown |

---

### 🌐 Network

| Command | What It Does |
|---|---|
| `ipconfig` | IP, subnet mask, gateway |
| `ipconfig /all` | MAC, DHCP, DNS, lease time |
| `ping example.com` | Test if target is reachable |
| `tracert example.com` | Trace route — all hops to target |
| `nslookup example.com` | Domain → IP (default DNS) |
| `nslookup example.com 1.1.1.1` | Domain → IP (custom DNS) |
| `netstat` | Active connections only |
| `netstat -abon` | All ports + PID + process name |

---

### 📁 Directories

| Command | What It Does |
|---|---|
| `cd` | Show current path |
| `cd [folder]` | Enter a folder |
| `cd ..` | Go one level up |
| `dir` | List files & folders |
| `dir /a` | Include hidden files |
| `dir /s` | Include all subdirectories |
| `tree` | Visual folder structure |
| `mkdir [name]` | Create new directory |
| `rmdir [name]` | Delete directory |

---

### 📄 Files

| Command | What It Does |
|---|---|
| `type file.txt` | Print file to terminal |
| `more file.txt` | View file page by page |
| `copy a.txt b.txt` | Copy file |
| `copy *.md C:\dest` | Copy all .md files (wildcard) |
| `move file.txt ..` | Move file |
| `del file.txt` | Delete file |
| `erase file.txt` | Same as del |

---

### ⚙️ Processes

| Command | What It Does |
|---|---|
| `tasklist` | All running processes + PID |
| `tasklist /FI "imagename eq sshd.exe"` | Filter by process name |
| `taskkill /PID 4567` | Kill process by PID |

---

### 🔁 Pipe Trick

```cmd
systeminfo | more        # View long output page by page
driverquery | more       # Same for drivers
some_cmd | more          # Works with any long output
```

---

---

## 🔗 References

- [TryHackMe — Windows Command Line Room](https://tryhackme.com/room/windowscommandline)
- [Microsoft Docs — CMD Reference](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands)

---

*Documented by **Muhammad Ali** — [TryHackMe Profile](https://tryhackme.com/p/Muhammad.Ali12)*
