# 🕵️ Burp Suite Basics — The Heart of Web App Hacking

> **Room:** Burp Suite: The Basics | **Path:** Jr Penetration Tester  
> **Difficulty:** Easy | **Platform:** TryHackMe

---

## 📌 Why This Room Matters

If you want to do web application pentesting, there is no escaping **Burp Suite**.  
It is the tool that **every professional penetration tester** uses.  
It is the industry standard in this field — and in this room we start from the very basics. 🔥

---

## 🗂️ Table of Contents

| # | Topic |
|---|-------|
| 1 | [What is Burp Suite?](#-what-is-burp-suite) |
| 2 | [Editions](#-editions) |
| 3 | [6 Main Tools](#-6-main-tools-easy-breakdown) |
| 4 | [Installation & Setup](#-installation-setup) |
| 5 | [Dashboard Navigation](#-dashboard-navigation) |
| 6 | [UI Overview — Every Tab Explained](#-ui-overview-every-tab-explained) |
| 7 | [Burp Proxy — The Core Feature](#-burp-proxy-the-core-feature) |
| 8 | [FoxyProxy Setup](#-foxyproxy-setup) |
| 9 | [Site Map](#-site-map-burps-diary-feature) |
| 10 | [How It Helps in Pentesting](#-how-it-helps-in-pentesting) |



---

## 🤔 What is Burp Suite?

> **In One Line:**  
> *A middleman tool that sits between your browser and the website — letting you see and modify all data passing through.*

**Burp Suite** is a Java-based framework used for web application penetration testing.  
It was built by PortSwigger and remains the **#1 industry standard tool** for web security testing.

```
Browser  ──────►  Burp Suite  ──────►  Website
                  (Sitting in the middle,
                   watching everything)
```

> ⚠️ **Note:** Burp Suite is **Java-based** — not a JavaScript framework. Don't get confused by the word "framework"!

---

## 📦 Editions

| Edition | Cost | Who It's For |
|---------|------|--------------|
| **Community** | Free ✅ | Students, Beginners, CTF Players |
| **Professional** | Paid 💰 | Bug Bounty Hunters, Professional Pentesters |
| **Enterprise** | Server Install 🏢 | Companies, Automated CI/CD Scanning |

> 🎯 We will work with **Community Edition** — and it is more than enough for this room!

---

## 🛠️ 6 Main Tools — Easy Breakdown

| Tool | One Line | Real Use Case |
|------|----------|---------------|
| 🔵 **Proxy** | Intercept traffic | View and modify requests/responses |
| 🔄 **Repeater** | Send a request again and again | Try SQLi payloads, test endpoints |
| 💥 **Intruder** | Spray requests automatically | Brute force, fuzzing |
| 🔑 **Decoder** | Encode/decode data | Base64, URL encoding |
| ⚖️ **Comparer** | Compare two things | Valid login vs Invalid login response |
| 🎲 **Sequencer** | Check randomness | How random are session cookies? |

---

## 💻 Installation & Setup

### Step 1 — Download Burp Suite
```bash
# Download from official site:
# https://portswigger.net/burp/communitydownload

# Install on Linux:
chmod +x burpsuite_community_linux_*.sh
./burpsuite_community_linux_*.sh
```

### Step 2 — First Launch

```
Open Burp Suite
        ↓
Accept Terms & Conditions
        ↓
Project Type → Select "Temporary Project" → Next
        ↓
Configuration → "Use Burp Defaults" → Start Burp
        ↓
🎯 Burp Dashboard is open!
```

> 💡 **Pro Tip:** If a training screen appears, make sure to read it when you have time — PortSwigger's documentation is world-class!

---

## 🧭 Dashboard Navigation

After launching Burp, you see **4 quadrants** on the dashboard:

```
┌─────────────────────┬─────────────────────┐
│    1. TASKS         │   2. EVENT LOG       │
│  Define background  │  What Burp did —     │
│  tasks here         │  all logs here       │
├─────────────────────┼─────────────────────┤
│  3. ISSUE ACTIVITY  │   4. ADVISORY        │
│  ❌ Pro Only        │  ❌ Pro Only         │
│  Vuln scanner       │  Fix suggestions     │
└─────────────────────┴─────────────────────┘
```

| # | Section | What It Does | Available in Community? |
|---|---------|--------------|------------------------|
| 1 | **Tasks** | Define background tasks — default "Live Passive Crawl" logs all visited pages | ✅ Yes |
| 2 | **Event Log** | Records proxy start, connections, and all actions | ✅ Yes |
| 3 | **Issue Activity** | Automated scanner vulnerabilities with severity levels | ❌ Pro Only |
| 4 | **Advisory** | Vulnerability details, references, fix suggestions, export to report | ❌ Pro Only |

---

## 🖥️ UI Overview — Every Tab Explained

The **top navigation bar** in Burp Suite has these tabs:

| Tab | What It Is |
|-----|-----------|
| **Dashboard** | Main overview — tasks, logs, issues |
| **Target** | Define scope, view site map |
| **Proxy** | Intercept traffic — you will use this the most |
| **Intruder** | Automated attack payloads |
| **Repeater** | Manually resend and modify requests |
| **Sequencer** | Analyze token randomness |
| **Decoder** | Encoding/Decoding utility |
| **Comparer** | Compare two responses side by side |
| **Logger** | Log of all HTTP traffic |
| **Extensions** | Install plugins from the BApp Store |

> 🔧 **Settings Button** (top right): Change proxy listener port, manage SSL certificates, and configure everything else from here.

### ⚙️ Important Settings to Configure First

```
Settings → Proxy → Listeners → Default: 127.0.0.1:8080
Settings → Proxy → Intercept → Intercept responses based on rules
Settings → Network → Connections → Upstream Proxy (if needed)
```

> 💡 The default proxy port is **8080** — you need to route your browser's traffic to this exact port!

---

## 🔵 Burp Proxy — The Core Feature

> *"This is the heart of Burp Suite"* 💙

### How the Proxy Works

**Without Proxy:**
```
Browser ──────────────────────► Website
```

**With Proxy:**
```
Browser ──► Burp Proxy ──► Website
              ↑
         Here you can:
         ✅ Stop the request
         ✅ Read it
         ✅ Modify it
         ✅ Forward or Drop it
```

### 3 Sub-Tabs Inside the Proxy Tab

| Sub-Tab | Purpose |
|---------|---------|
| **Intercept** | Stop live traffic and inspect it — on/off toggle |
| **HTTP History** | Record of all past requests |
| **WebSockets History** | Log of WebSocket connections |

### Intercept Controls

```
[Forward]              → Send the request to the server
[Drop]                 → Delete the request — server never sees it
[Intercept is ON/OFF]  → Toggle interception on or off
[Action]               → Send to Repeater / Intruder / Decoder etc.
```

---

## 🦊 FoxyProxy Setup

To route browser traffic through Burp, we use the **FoxyProxy** extension.

### Step-by-Step

```
Step 1: Install FoxyProxy Standard in Firefox
        (from Firefox Add-ons page)
        ↓
Step 2: Click FoxyProxy icon → Options
        ↓
Step 3: Click "Add" to create a new proxy:
        Title:    Burp
        Protocol: HTTP  ← only select this one!
        Host:     127.0.0.1
        Port:     8080
        ↓
Step 4: Save it
        ↓
Step 5: Enable "Burp" from the FoxyProxy menu
        ↓
Step 6: In Burp Suite → Proxy → Intercept → turn "Intercept is on"
        ↓
🎉 Ready to intercept!
```

> ⚠️ **Important:** After you finish, always turn FoxyProxy **OFF** — otherwise normal browsing will not work!

---

## 🗺️ Site Map — Burp's Diary Feature

### Simple Explanation

When you browse a website with Burp proxy on — Burp **automatically records** every page you visit.

All of this appears in a **tree structure**:

```
🌐 10.114.176.205
├── 📁 /
├── 📄 /about
├── 📄 /contact
├── 📄 /products
├── 📄 /ticket
└── 🔴 /5yjR2GLcoGoij2ZK  ← Suspicious! What is this?
```

### Real Life Analogy

> *Imagine I am a security guard in a building, noting down every door that gets opened.*
>
> Room 1 ✅ okay | Room 2 ✅ okay | **Secret Room 🔴 Interesting!**
>
> **Burp = Security Guard | Site Map = His Diary** 📓

### Where to Find Site Map

```
Target Tab → Site Map → Tree view of all visited pages
```

> 💡 Use the **Filter** to show only your target site — there will be a lot of entries if FoxyProxy was on while you browsed other sites!

---

## 🎯 How It Helps in Pentesting

| Benefit | Why It Matters |
|---------|---------------|
| 🕵️ **Discover hidden pages** | Pages that don't appear in normal navigation |
| 🔌 **Capture API endpoints** | Developers hide these — Burp finds them automatically |
| 🗺️ **Map the full website structure** | Without manually guessing paths |
| 🔍 **Useful in the recon phase** | First step to understanding the target |
| 📋 **Request history** | Send any interesting request to Repeater later |

> **In Short:**  
> *Site Map = An automatic map of the website that Burp builds for you as you browse* 🗺️

---

## ✅ Key Takeaways

```
✔️  Burp Suite = Java-based web pentesting framework
✔️  Community Edition = Enough for students and beginners
✔️  Proxy = The core tool — all traffic passes through it
✔️  FoxyProxy = Connects your browser to Burp (Port 8080)
✔️  Site Map = Automatic website structure mapper
✔️  Intercept ON/OFF = Stop or let traffic through
✔️  Repeater = Resend and modify a request as many times as needed
```

---

## 🔗 Resources

- 🌐 [TryHackMe Room](https://tryhackme.com/room/burpsuitebasics)
- 📚 [PortSwigger Official Docs](https://portswigger.net/burp/documentation)
- 🎓 [PortSwigger Web Security Academy](https://portswigger.net/web-security) — Free and the best!

---

*Made with 🖤 | Muhammad Ali | TryHackMe: [Muhammad.Ali12](https://tryhackme.com/p/Muhammad.Ali12)*
