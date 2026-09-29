# 🌐 Networking Basics

> **Author of notes:** Muhammad Ali (`Muhammad.Ali12`)
> **Level:** Beginner 🟢
> **Goal:** Understand how computers talk to each other, in simple words, with tools you can try yourself.

---

## 📑 Table of Contents

- [1. 🧠 What is a Network?](#1--what-is-a-network)
- [2. 🗺️ Types of Networks](#2-️-types-of-networks)
- [3. 🔧 Network Devices](#3--network-devices)
- [4. 🏷️ IP Address](#4-️-ip-address)
- [5. 🪪 MAC Address](#5--mac-address)
- [6. 🚪 Ports](#6--ports)
- [7. 📖 DNS](#7--dns)
- [8. 🎟️ DHCP](#8-️-dhcp)
- [9. 🤝 TCP vs UDP](#9--tcp-vs-udp)
- [10. 🧱 OSI and TCP/IP Models](#10--osi-and-tcpip-models)
- [11. 📨 Common Protocols](#11--common-protocols)
- [12. ✂️ Subnetting (Simple Intro)](#12-️-subnetting-simple-intro)
- [13. 🔀 NAT](#13--nat)
- [14. 🛠️ Useful Commands](#14-️-useful-commands)
- [15. 🛡️ Security View](#15-️-security-view)
- [16. 🧾 Cheat Sheet](#16--cheat-sheet)
- [17. 💡 Key Takeaways](#17--key-takeaways)

---

## 1. 🧠 What is a Network?

A **network** is two or more devices connected so they can share data.

**Analogy:** A network is like a **postal system** 📮.

| Postal System | Network |
|---------------|---------|
| House address | IP address |
| Person's name / ID | MAC address |
| Room number in the house | Port |
| Post office | Router |
| Phone book | DNS |
| Letter | Data packet |

```
 💻 Laptop ───┐
 📱 Phone  ───┼───► 🛜 Router ───► 🌍 Internet
 🖥️ PC     ───┘
```

📸 **Screenshot:** _Your home network devices_
`![Home Network](screenshots/home-network.png)`

---

## 2. 🗺️ Types of Networks

| Type | Full Name | Size | Example |
|------|-----------|------|---------|
| **PAN** | Personal Area Network | Tiny | Phone + Bluetooth headphones |
| **LAN** | Local Area Network | Home / office | Your Wi-Fi at home |
| **MAN** | Metropolitan Area Network | City | City-wide cable network |
| **WAN** | Wide Area Network | Country / world | The Internet |

> 💡 **The Internet** is just a giant WAN made of many smaller networks joined together.

---

## 3. 🔧 Network Devices

| Device | Job | Analogy |
|--------|-----|---------|
| 🔌 **Hub** | Sends data to everyone | Shouting in a room 📢 |
| 🔀 **Switch** | Sends data only to the right device (inside a LAN) | Smart mail sorter inside a building |
| 🛜 **Router** | Connects different networks, finds the best path | Post office / road map 🗺️ |
| 🧱 **Firewall** | Allows or blocks traffic by rules | Security guard at the gate 💂 |
| 📡 **Access Point** | Gives Wi-Fi | A door that opens to the network |
| 🧭 **Modem** | Connects you to your ISP | Bridge to the outside world |

```
 Devices ──► Switch ──► Router ──► Modem ──► ISP ──► Internet
```

---

## 4. 🏷️ IP Address

An **IP address** is the **home address** of a device on a network.

### IPv4 vs IPv6

| Item | IPv4 | IPv6 |
|------|------|------|
| Example | `192.168.1.10` | `2001:db8::1` |
| Size | 32 bits | 128 bits |
| Total addresses | About 4.3 billion | Almost endless |
| Status | Still most used | Growing |

### Public vs Private IP

| Type | Range | Where used |
|------|-------|------------|
| 🏠 Private | `10.0.0.0 – 10.255.255.255` | Inside home / office |
| 🏠 Private | `172.16.0.0 – 172.31.255.255` | Inside home / office |
| 🏠 Private | `192.168.0.0 – 192.168.255.255` | Inside home / office |
| 🌍 Public | Everything else | On the Internet |

> 🔁 `127.0.0.1` is **localhost**. It means "this same computer".

```
 192  .  168  .   1   .   10
 └─────┬─────┘   └────┬────┘
   Network part     Host part
```

---

## 5. 🪪 MAC Address

A **MAC address** is the **hardware ID** of your network card. Like a **car chassis number** 🚗. It is set by the manufacturer.

```
 Example:  A4:5E:60:C2:9F:1B
           └──┬───┘ └──┬───┘
        Maker (vendor)  Device number
```

| Item | IP Address | MAC Address |
|------|-----------|-------------|
| Works at | Network level (Layer 3) | Local link level (Layer 2) |
| Changes? | Yes, can change | Fixed (but can be spoofed) |
| Used for | Travel across networks | Talk inside one LAN |
| Analogy | Home address | ID card number |

**ARP** (Address Resolution Protocol) finds the MAC address for a given IP inside a LAN.
"Who has 192.168.1.5? Tell me your MAC!" 📣

---

## 6. 🚪 Ports

**Analogy:** An IP address is a **building**. Ports are the **doors** 🚪. Each service uses its own door.

There are **65,535** ports.

| Range | Name |
|-------|------|
| 0 – 1023 | Well-known ports |
| 1024 – 49151 | Registered ports |
| 49152 – 65535 | Dynamic / private ports |

### Common ports

| Port | Service | Use |
|------|---------|-----|
| 20 / 21 | FTP | File transfer |
| 22 | SSH | Secure remote login |
| 23 | Telnet | Old remote login (not secure) |
| 25 | SMTP | Send email |
| 53 | DNS | Name lookup |
| 80 | HTTP | Websites |
| 110 | POP3 | Receive email |
| 143 | IMAP | Receive email |
| 443 | HTTPS | Secure websites |
| 445 | SMB | Windows file sharing |
| 3306 | MySQL | Database |
| 3389 | RDP | Windows remote desktop |

---

## 7. 📖 DNS

**DNS** (Domain Name System) turns names into IP addresses.

**Analogy:** DNS is the **phone book** 📖. You remember "Ali", the phone book gives the number.

```
 You type: google.com
      │
      ▼
 🧑 Browser ──► 📖 DNS Server ──► "google.com = 142.250.x.x"
      │
      ▼
 🌍 Connects to that IP
```

### Common DNS record types

| Record | Meaning | Example |
|--------|---------|---------|
| `A` | Name ➜ IPv4 | `site.com → 1.2.3.4` |
| `AAAA` | Name ➜ IPv6 | `site.com → 2001:...` |
| `CNAME` | Alias to another name | `www → site.com` |
| `MX` | Mail server | Where email goes |
| `TXT` | Extra text info | SPF, verification |
| `NS` | Name server | Who manages the domain |

```bash
nslookup google.com
dig google.com
```

---

## 8. 🎟️ DHCP

**DHCP** gives IP addresses to devices **automatically**. Like a **hotel reception** 🏨 giving you a room number when you arrive.

### The DORA process

```
 💻 Client                     🖥️ DHCP Server
    │  D - Discover  ("Anyone here? I need an IP!") │
    │ ────────────────────────────────────────────► │
    │  O - Offer     ("Here, take 192.168.1.20")    │
    │ ◄──────────────────────────────────────────── │
    │  R - Request   ("Yes, I want that one")       │
    │ ────────────────────────────────────────────► │
    │  A - Acknowledge ("Done, it is yours")        │
    │ ◄──────────────────────────────────────────── │
```

---

## 9. 🤝 TCP vs UDP

| Item | TCP | UDP |
|------|-----|-----|
| Full name | Transmission Control Protocol | User Datagram Protocol |
| Connection | Yes (handshake first) | No |
| Reliable? | ✅ Yes, checks every packet | ❌ No checks |
| Speed | Slower | Faster |
| Analogy | Phone call, confirm every sentence ☎️ | Radio broadcast, just talk 📻 |
| Used for | Web, email, file transfer | Video calls, gaming, DNS |

### TCP Three-Way Handshake

```
 💻 Client                 🖥️ Server
    │ ── SYN ─────────────► │   "Can we talk?"
    │ ◄──────── SYN/ACK ─── │   "Yes, can you hear me?"
    │ ── ACK ─────────────► │   "Yes, let's begin."
```

---

## 10. 🧱 OSI and TCP/IP Models

Models split networking into **layers**, like a **layered cake** 🍰. Each layer has one job.

### OSI Model (7 layers)

| # | Layer | Job | Example |
|---|-------|-----|---------|
| 7 | Application | What the user sees | HTTP, DNS, FTP |
| 6 | Presentation | Format, encrypt, compress | TLS, JPEG |
| 5 | Session | Start / keep / end sessions | Login sessions |
| 4 | Transport | Ports, reliable delivery | TCP, UDP |
| 3 | Network | IP addresses, routing | IP, Router |
| 2 | Data Link | MAC addresses, frames | Switch, ARP |
| 1 | Physical | Cables, signals | Ethernet, Wi-Fi |

> 🧠 **Memory trick (top to bottom):** **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing.

### OSI vs TCP/IP

```
   OSI Model              TCP/IP Model
 ┌────────────────┐
 │ 7 Application  │
 │ 6 Presentation │ ─────► Application
 │ 5 Session      │
 ├────────────────┤
 │ 4 Transport    │ ─────► Transport
 ├────────────────┤
 │ 3 Network      │ ─────► Internet
 ├────────────────┤
 │ 2 Data Link    │
 │ 1 Physical     │ ─────► Network Access
 └────────────────┘
```

### Encapsulation (packing the parcel 📦)

```
 Data ─► + Header(TCP) ─► + Header(IP) ─► + Header(MAC) ─► Bits on the wire
        (Segment)         (Packet)         (Frame)
```

---

## 11. 📨 Common Protocols

| Protocol | Port | Job | Secure? |
|----------|------|-----|---------|
| HTTP | 80 | Web pages | ❌ |
| HTTPS | 443 | Web pages with encryption | ✅ |
| FTP | 21 | File transfer | ❌ |
| SFTP | 22 | File transfer over SSH | ✅ |
| SSH | 22 | Remote login | ✅ |
| Telnet | 23 | Remote login | ❌ |
| SMTP | 25 | Send email | ❌ (unless TLS) |
| DNS | 53 | Name lookup | ❌ (unless DoH/DoT) |
| SMB | 445 | Windows file sharing | Depends |

> 💡 If a protocol sends **plain text**, anyone on the network can read it. Prefer secure versions.

---

## 12. ✂️ Subnetting (Simple Intro)

**Subnetting** splits one big network into smaller ones. Like dividing a big building into **floors** 🏢.

### CIDR notation

`192.168.1.0/24` means the first **24 bits** are the network. The rest are for hosts.

| CIDR | Subnet Mask | Usable Hosts |
|------|-------------|--------------|
| `/8` | 255.0.0.0 | 16,777,214 |
| `/16` | 255.255.0.0 | 65,534 |
| `/24` | 255.255.255.0 | 254 |
| `/30` | 255.255.255.252 | 2 |

```
 192.168.1.0/24
 ├─ Network address : 192.168.1.0    (name of the network)
 ├─ First host      : 192.168.1.1
 ├─ Last host       : 192.168.1.254
 └─ Broadcast       : 192.168.1.255  (talks to everyone)
```

**Default gateway** = the router's IP. It is the exit door 🚪 to other networks.

---

## 13. 🔀 NAT

**NAT** (Network Address Translation) lets many private devices share **one public IP**.

**Analogy:** An office with one phone number ☎️ and a receptionist who forwards calls to the right desk.

```
 💻 192.168.1.10 ─┐
 📱 192.168.1.11 ─┼─► 🛜 Router (NAT) ─► Public IP 103.x.x.x ─► 🌍 Internet
 🖥️ 192.168.1.12 ─┘
```

---

## 14. 🛠️ Useful Commands

| Goal | Linux / Mac | Windows |
|------|-------------|---------|
| Show my IP | `ip a` | `ipconfig` |
| Test connection | `ping 8.8.8.8` | `ping 8.8.8.8` |
| Trace the path | `traceroute google.com` | `tracert google.com` |
| DNS lookup | `dig google.com` | `nslookup google.com` |
| See open connections | `ss -tuln` | `netstat -ano` |
| See ARP table | `ip neigh` | `arp -a` |
| Scan ports | `nmap 192.168.1.1` | `nmap 192.168.1.1` |
| Capture packets | `tcpdump` / Wireshark | Wireshark |

```bash
ping -c 4 google.com          # send 4 pings
traceroute google.com         # see every hop
nmap -p 22,80,443 TARGET_IP   # scan specific ports
```

📸 **Screenshot:** _ping and traceroute output_
`![Ping Traceroute](screenshots/ping-traceroute.png)`

---

## 15. 🛡️ Security View

| Concept | Why a hacker cares | Why a defender cares |
|---------|--------------------|----------------------|
| Open ports | Entry doors to attack | Close what you do not need |
| Plain-text protocols (HTTP, FTP, Telnet) | Sniff passwords | Use HTTPS, SFTP, SSH |
| DNS | Find subdomains, spoof answers | Monitor for odd lookups |
| ARP | ARP spoofing, man-in-the-middle | Use dynamic ARP inspection |
| Firewall rules | Find gaps | Block by default, allow by need |
| Wi-Fi | Weak passwords, rogue access points | Use WPA2/WPA3, strong keys |

> 🧭 **Pentest flow:** Find live hosts ➜ scan ports ➜ find services ➜ check versions ➜ look for weak spots.

---

## 16. 🧾 Cheat Sheet

| Term | One-line meaning |
|------|------------------|
| LAN / WAN | Small local network / big wide network |
| IP | Address of a device |
| MAC | Hardware ID of network card |
| Port | Door to a service |
| DNS | Name ➜ IP |
| DHCP | Gives IPs automatically |
| TCP | Reliable, with handshake |
| UDP | Fast, no checks |
| Router | Connects networks |
| Switch | Connects devices in a LAN |
| Firewall | Allows or blocks traffic |
| NAT | Many private IPs, one public IP |
| Gateway | Exit door of a network |
| CIDR `/24` | 254 usable hosts |

---

## 17. 💡 Key Takeaways

- ✅ IP = address, MAC = ID, Port = door.
- ✅ DNS turns names into IPs. DHCP hands out IPs.
- ✅ TCP is careful, UDP is fast.
- ✅ The OSI model has 7 layers. Learn them top to bottom.
- ✅ Private IPs stay inside. NAT shares one public IP.
- ✅ Plain-text protocols are unsafe. Use secure ones.
- ✅ Learn `ping`, `traceroute`, `nmap`, and Wireshark by practice.

---

⭐ _Notes written while learning networking for cybersecurity. Practice every command yourself._
