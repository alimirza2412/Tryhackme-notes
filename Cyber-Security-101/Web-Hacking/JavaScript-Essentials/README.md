# 📘 JavaScript Essentials — Web Security & Core Concepts

> Personal notes on JavaScript fundamentals with a focus on **how attackers exploit JS** and how to prevent it.  
> Based on TryHackMe learning path.

---

## 📚 Table of Contents

- [JavaScript Overview](#-javascript-overview)
- [Variables & Data Types](#-variables--data-types)
- [Functions & Loops](#-functions--loops)
- [Integrating JavaScript in HTML](#-integrating-javascript-in-html)
- [Abusing Dialogue Functions](#-abusing-dialogue-functions)
- [Bypassing Control Flow Statements](#-bypassing-control-flow-statements)
- [Exploring Minified Files](#-exploring-minified-files)
- [Best Practices](#-best-practices)

---

## 🌐 JavaScript Overview

JavaScript is an **interpreted language** — meaning the browser (Chrome, Firefox, etc.) reads and executes the code directly without needing to compile it first.

```javascript
// First Program
console.log("Hello World");

// Variable and Data Type
let age = 24;

// Control Flow Statement
if (age >= 18) {
  console.log("You are an adult.");
} else {
  console.log("You are a minor.");
}

// Function
function greet(name) {
  console.log("Hello, " + name + "!");
}

greet("Bob");
```

---

## 📦 Variables & Data Types

Variables are containers that store data values — think of labeling a bucket so you can easily reference it later.

### Variable Declaration

| Keyword | Description |
|---------|-------------|
| `var`   | Old method — avoid using it |
| `let`   | Modern method — use when the value needs to change |
| `const` | Modern method — use when the value stays fixed |

### Data Types

- `String` — text values
- `Number` — numeric values
- `Boolean` — true / false

---

## 🔁 Functions & Loops

A **function** is a block of code designed to perform a specific task. When you need to do the same thing multiple times, put it in a function.

A **loop** runs a block of code repeatedly as long as a condition is true.

```javascript
function PrintResult(rollNum) {
  alert("Username with roll number " + rollNum + " has passed the exam");
}

const rollNumbers = [101, 102, 103];

for (let i = 0; i < rollNumbers.length; i++) {
  PrintResult(rollNumbers[i]);
}
```

---

## 🔗 Integrating JavaScript in HTML

There are two ways to integrate JavaScript into HTML:

### 1. Internal JavaScript

JavaScript is written inside the same HTML file using the `<script>` tag.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Internal JS</title>
</head>
<body>
  <h1>Addition of Two Numbers</h1>
  <p id="result"></p>

  <script>
    let x = 5;
    let y = 10;
    let result = x + y;
    document.getElementById("result").innerHTML = "The result is: " + result;
  </script>
</body>
</html>
```

### 2. External JavaScript

JavaScript is written in a separate `.js` file and linked to the HTML file.

```html
<!-- HTML File -->
<script src="script.js"></script>
```

> ✅ Real websites and large projects use **External JS** — HTML renders first and JavaScript loads separately for better performance.

---

## 🚨 Abusing Dialogue Functions

**Concept:** An attacker uses legitimate dialogue boxes (pop-ups, prompts) to trick users into performing harmful actions. Since these pop-ups look like part of real software, users tend to trust them.

JavaScript provides 3 built-in dialogue functions:

| Function    | Description |
|-------------|-------------|
| `alert()`   | Shows a message — user can only click OK |
| `prompt()`  | Takes input from the user |
| `confirm()` | Shows two options — OK (true) or Cancel (false) |

```javascript
// Alert
alert("Hello User");

// Prompt
let name = prompt("What is your name?");
alert("Hello " + name);

// Confirm
confirm("Are you sure?");
```

### ⚠️ How Hackers Exploit This — DoS Attack Example

An attacker sends an HTML file via email. When the user opens it, a popup appears 500 times — this is a simple **Denial of Service (DoS)** attack.

```html
<script>
  for (let i = 0; i < 500; i++) {
    alert("Your device is hacked!");
  }
</script>
```

---

## 🔓 Bypassing Control Flow Statements

Control flow statements like `if/else`, `switch`, and loops control how code executes. Attackers can bypass these when they are implemented only on the client side.

### Age Verification Example

```html
<script>
  let age = prompt("What is your age?");

  if (age >= 18) {
    document.getElementById("message").innerHTML = "You are an adult.";
  } else {
    document.getElementById("message").innerHTML = "You are a minor.";
  }
</script>
```

### ❌ Insecure Login — Client-Side Only

```javascript
if (username === "admin" && password === "1234") {
  alert("Logged in!");
} else {
  alert("Wrong credentials");
}
```

**Why this is dangerous:**
- The password is visible in JS — anyone can use "View Source" in the browser to see it
- A hacker can directly change variables in the browser console
- There is no server-side verification at all

### ✅ Security Lesson

> Sensitive checks like login and password validation must **never** be written in client-side JavaScript.  
> Always use **server-side authentication** — PHP, Python, Node.js, etc.

---

## 📦 Exploring Minified Files

**Minification** removes spaces, line breaks, and comments from JS files to reduce file size and make the browser load them faster.

```javascript
// Normal Code — Human Readable
function hi() {
  alert("Welcome to THM");
}
hi();

// Minified — Same functionality, smaller file
function hi(){alert("Welcome to THM")}hi();
```

### 🔒 Obfuscation

Obfuscation intentionally makes code difficult for humans to read and understand — essentially encrypting the logic.

```javascript
// Original
function hi() {
  alert("Welcome to THM");
}

// Obfuscated — same functionality, but hard to understand
(function(_0x114713,_0x2246f2){var _0x51a830=_0x33bf...
```

> 🛠️ There are online tools available to both **obfuscate** and **deobfuscate** JavaScript code.

---

## 🛡️ Best Practices

| Practice | Why It Matters |
|----------|----------------|
| **Never rely only on client-side validation** | Hackers can view and manipulate JS directly in the browser |
| **Avoid unverified third-party libraries** | A malicious library can gain full control of your website |
| **Never write secrets in code** | Passwords and API keys are visible via View Source |
| **Always minify and obfuscate for production** | Adds a layer of protection to your source code |

---

## 🧠 Key Takeaways

- JavaScript runs in the browser — **attackers can access it directly**
- Login and sensitive logic must always live on the **server side**
- Minification is for **performance**, not security
- Obfuscation makes code **harder** to read, not impossible

---

> 📌 *These notes are based on the TryHackMe JavaScript Essentials module.*  
> 🔗 [TryHackMe Profile](https://tryhackme.com)
