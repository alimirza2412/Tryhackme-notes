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

JavaScript ek **interpreted language** hai — matlab browser (Chrome, Firefox, etc.) code ko seedha read karke execute kar deta hai, pehle compile karne ki zaroorat nahi hoti.

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

Variables ek container ki tarah hain jo data store karte hain — jaise kisi bucket par label lagana taake baad mein aasaani se refer kar sako.

### Variable Declaration

| Keyword | Description |
|---------|-------------|
| `var`   | Old method — avoid karna chahiye |
| `let`   | Modern method — jab value change karni ho |
| `const` | Modern method — jab value fix ho, change na karni ho |

### Data Types

- `String` — text values
- `Number` — numeric values
- `Boolean` — true / false

---

## 🔁 Functions & Loops

**Function** ek block of code hai jo ek specific kaam karta hai. Jab same kaam baar baar karna ho, toh function banao.

**Loop** ek code block ko baar baar run karta hai jab tak condition true ho.

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

JavaScript ko HTML mein do tarikon se integrate kar sakte hain:

### 1. Internal JavaScript

JavaScript same HTML file mein `<script>` tag ke andar likhi jaati hai.

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

JavaScript alag `.js` file mein likhi jaati hai aur HTML usse link karta hai.

```html
<!-- HTML File -->
<script src="script.js"></script>
```

> ✅ Real websites aur large projects mein **External JS** use hoti hai — HTML pehle render hoti hai aur JS alag se load hoti hai better performance ke liye.

---

## 🚨 Abusing Dialogue Functions

**Concept:** Attacker legitimate dialogue boxes (pop-ups, prompts) ko use karta hai taake user ko trick karke koi harmful action karwa sake. Ye pop-ups asli software ka part lagte hain, isliye user trust kar leta hai.

JavaScript 3 built-in dialogue functions provide karta hai:

| Function    | Description |
|-------------|-------------|
| `alert()`   | Sirf ek message dikhata hai — user sirf OK click kar sakta hai |
| `prompt()`  | User se input leta hai |
| `confirm()` | Do options deta hai — OK (true) ya Cancel (false) |

```javascript
// Alert
alert("Hello User");

// Prompt
let name = prompt("What is your name?");
alert("Hello " + name);

// Confirm
confirm("Are you sure?");
```

### ⚠️ How Hackers Exploit This — DOS Attack Example

Attacker ek HTML file email karta hai. User usse open karta hai aur 500 baar popup aata hai — yeh ek simple **Denial of Service (DoS)** attack hai.

```html
<script>
  for (let i = 0; i < 500; i++) {
    alert("Your device is hacked!");
  }
</script>
```

---

## 🔓 Bypassing Control Flow Statements

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

**Problems:**
- Password JS mein visible hai — browser ka "View Source" se dekha ja sakta hai
- Hacker browser console mein variable directly change kar sakta hai
- Koi server-side verification nahi hai

### ✅ Security Lesson

> Sensitive data (login/password checks) kabhi bhi client-side JS mein nahi likhni chahiye.  
> Hamesha **server-side authentication** use karo — PHP, Python, Node.js, etc.

---

## 📦 Exploring Minified Files

**Minification:** JS file se spaces, line breaks, aur comments hata diye jaate hain taake file ka size chhota ho aur browser mein jaldi load ho.

```javascript
// Normal Code — Human Readable
function hi() {
  alert("Welcome to THM");
}
hi();

// Minified — Same kaam, chhoti file
function hi(){alert("Welcome to THM")}hi();
```

### 🔒 Obfuscation

Code ko intentionally mushkil banaya jaata hai taake insaan aasaani se read na kar sake — ek tarah ka encryption.

```javascript
// Original
function hi() {
  alert("Welcome to THM");
}

// Obfuscated — same kaam, lekin samajhna mushkil
(function(_0x114713,_0x2246f2){var _0x51a830=_0x33bf...
```

> 🛠️ Tools: Websites available hain jo JS ko obfuscate aur deobfuscate karte hain.

---

## 🛡️ Best Practices

| Practice | Why It Matters |
|----------|----------------|
| **Client-side validation par rely mat karo** | Hacker browser mein JS dekh aur manipulate kar sakta hai |
| **Unverified libraries mat use karo** | Malicious library puri website ka control le sakti hai |
| **Secrets (passwords, API keys) code mein mat likho** | View Source se visible ho jaate hain |
| **Production mein hamesha Minify aur Obfuscate karo** | Source code ko thoda protect karta hai |

---

## 🧠 Key Takeaways

- JavaScript browser mein run hoti hai — **attacker directly access kar sakta hai**
- Login aur sensitive logic hamesha **server-side** par ho
- Minification performance ke liye hai, security ke liye nahi
- Obfuscation code ko harder banata hai, impossible nahi

---

> 📌 *Ye notes TryHackMe JavaScript Essentials module par based hain.*  
> 🔗 Profile: [TryHackMe](https://tryhackme.com)
