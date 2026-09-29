# 🔐 Cryptography Basics

> **Author of notes:** Muhammad Ali (`Muhammad.Ali12`)
> **Level:** Beginner 🟢
> **Goal:** Understand how data is kept secret, checked, and trusted, in simple words.

---

## 📑 Table of Contents

- [1. 🧠 What is Cryptography?](#1--what-is-cryptography)
- [2. 🔤 Key Terms](#2--key-terms)
- [3. 🎭 Encoding vs Encryption vs Hashing](#3--encoding-vs-encryption-vs-hashing)
- [4. 🗝️ Symmetric Encryption](#4-️-symmetric-encryption)
- [5. 🔑 Asymmetric Encryption](#5--asymmetric-encryption)
- [6. 🧬 Hashing](#6--hashing)
- [7. 🧂 Salting and Password Storage](#7--salting-and-password-storage)
- [8. ✍️ Digital Signatures](#8-️-digital-signatures)
- [9. 📜 Certificates and TLS](#9--certificates-and-tls)
- [10. 🤝 Key Exchange](#10--key-exchange)
- [11. 🏛️ Classic Ciphers](#11-️-classic-ciphers)
- [12. 💥 Common Attacks](#12--common-attacks)
- [13. 🛠️ Useful Tools and Commands](#13-️-useful-tools-and-commands)
- [14. 🧾 Cheat Sheet](#14--cheat-sheet)
- [15. 💡 Key Takeaways](#15--key-takeaways)

---

## 1. 🧠 What is Cryptography?

**Cryptography** is the science of protecting information so only the right people can read it or trust it.

**Analogy:** Cryptography is like a **locked box** 📦 and **wax seal** on a letter.
- The lock keeps the letter **secret**.
- The seal proves the letter was **not opened** and came from the **right sender**.

### The CIA + extra goals

| Goal | Meaning | Tool used |
|------|---------|-----------|
| 🤫 Confidentiality | Only the right people can read | Encryption |
| 🧱 Integrity | Data was not changed | Hashing, HMAC |
| ✅ Authenticity | It really came from the sender | Digital signatures |
| 🚫 Non-repudiation | Sender cannot deny sending | Digital signatures |

```
 Plaintext ──► 🔒 Encrypt (with key) ──► Ciphertext ──► 🔓 Decrypt (with key) ──► Plaintext
  "Hello"                                 "x8#Qz!"                                 "Hello"
```

---

## 2. 🔤 Key Terms

| Term | Meaning |
|------|---------|
| **Plaintext** | Original readable data |
| **Ciphertext** | Scrambled data after encryption |
| **Cipher / Algorithm** | The method used to scramble |
| **Key** | The secret value used with the algorithm |
| **Encryption** | Plaintext ➜ Ciphertext |
| **Decryption** | Ciphertext ➜ Plaintext |
| **Hash** | Fixed-size fingerprint of data |
| **Salt** | Random data added before hashing |
| **Cryptanalysis** | Trying to break a cipher |

> 🧠 **Kerckhoffs's principle:** A system should stay safe even if everyone knows the algorithm. **Only the key must be secret.**

---

## 3. 🎭 Encoding vs Encryption vs Hashing

Beginners mix these up a lot. 😅

| Item | Encoding | Encryption | Hashing |
|------|----------|------------|---------|
| Purpose | Change format | Keep secret | Check integrity, store passwords |
| Needs a key? | ❌ No | ✅ Yes | ❌ No |
| Reversible? | ✅ Yes, anyone can | ✅ Yes, with the key | ❌ No (one way) |
| Example | Base64, URL encoding | AES, RSA | SHA-256, bcrypt |
| Analogy | Translating English to Urdu 🌐 | Locking a box 🔒 | Taking a fingerprint 👆 |

> ⚠️ **Base64 is NOT encryption.** Anyone can decode it.

```bash
echo "hello" | base64          # aGVsbG8K
echo "aGVsbG8K" | base64 -d    # hello
```

---

## 4. 🗝️ Symmetric Encryption

**One key** is used to lock and unlock.

**Analogy:** A **house key** 🏠. The same key locks and unlocks the door. Problem: how do you safely give a copy to your friend?

```
 Alice ── "Hello" ──► 🔒 [ Key K ] ──► "x8#Qz" ──► 🔓 [ Key K ] ──► "Hello" ── Bob
                       (same key on both sides)
```

### Common algorithms

| Algorithm | Key size | Status |
|-----------|----------|--------|
| **AES** | 128 / 192 / 256 bits | ✅ Standard, widely used |
| **ChaCha20** | 256 bits | ✅ Modern, fast on phones |
| 3DES | 168 bits (effective less) | ⚠️ Old, avoid |
| DES | 56 bits | ❌ Broken |
| RC4 | Variable | ❌ Broken |

### Block vs Stream ciphers

| Type | How it works | Example |
|------|--------------|---------|
| Block | Encrypts data in fixed-size chunks | AES |
| Stream | Encrypts one bit / byte at a time | ChaCha20 |

### Cipher modes (for block ciphers)

| Mode | Note |
|------|------|
| ECB | ❌ Same input gives same output. Patterns leak. Avoid |
| CBC | Uses an IV, chains blocks |
| CTR | Turns block cipher into stream cipher |
| GCM | ✅ Encrypts **and** checks integrity. Best choice |

> **IV** (Initialization Vector) is a random starting value. It makes sure the same message gives different ciphertext each time.

**Pros:** Fast. **Cons:** Key sharing is hard.

---

## 5. 🔑 Asymmetric Encryption

**Two keys** are used: a **public key** and a **private key**.

**Analogy:** A **mailbox** 📮.
- Public key = the mail slot. Anyone can drop a letter in.
- Private key = the mailbox key. Only you can open it.

```
 Bob's Public Key (shared with everyone) 🔓
        │
 Alice ─┴─ "Hello" ─► 🔒 Encrypt with Bob's PUBLIC key ─► "x8#Qz"
                                                            │
 Bob ◄── "Hello" ◄── 🔓 Decrypt with Bob's PRIVATE key ◄────┘
```

### Common algorithms

| Algorithm | Based on | Use |
|-----------|----------|-----|
| **RSA** | Big prime numbers | Encryption, signatures (2048+ bits) |
| **ECC** (e.g. Curve25519) | Elliptic curves | Smaller keys, same safety |
| **Diffie-Hellman** | Math problem (discrete log) | Key exchange |
| **ElGamal** | Discrete log | Encryption |

### Symmetric vs Asymmetric

| Item | Symmetric | Asymmetric |
|------|-----------|------------|
| Keys | 1 shared key | Public + private pair |
| Speed | 🚀 Fast | 🐢 Slow |
| Key sharing | Hard | Easy (public key is public) |
| Best for | Large data | Small data, key exchange, signatures |
| Example | AES | RSA |

> 💡 **In real life we use both.** Asymmetric encryption shares a secret key safely, then symmetric encryption protects the data. This is what HTTPS does.

---

## 6. 🧬 Hashing

A **hash function** turns any data into a **fixed-size fingerprint**.

**Analogy:** A **meat grinder** 🥩. You can turn a steak into mince, but you cannot turn mince back into the steak.

```
 "hello"        ──► SHA-256 ──► 2cf24dba5fb0a30e...  (64 hex chars)
 "hello!"       ──► SHA-256 ──► ce06092fb948d9ff...  (totally different)
 A 5 GB movie   ──► SHA-256 ──► also 64 hex chars
```

### Rules of a good hash

| Rule | Meaning |
|------|---------|
| One-way | Cannot reverse the hash |
| Deterministic | Same input always gives same hash |
| Avalanche effect | Tiny change gives a totally different hash |
| Collision resistant | Very hard to find two inputs with the same hash |

### Common hashes

| Algorithm | Output size | Status |
|-----------|-------------|--------|
| MD5 | 128 bits (32 hex) | ❌ Broken (collisions) |
| SHA-1 | 160 bits (40 hex) | ❌ Broken (collisions) |
| SHA-256 | 256 bits (64 hex) | ✅ Good |
| SHA-512 | 512 bits (128 hex) | ✅ Good |
| SHA-3 | Variable | ✅ Good |

### Uses of hashing

- ✅ Check a downloaded file was not changed
- ✅ Store passwords (with a slow hash and salt)
- ✅ Digital signatures and blockchains

```bash
echo -n "hello" | sha256sum
sha256sum file.iso
```

### HMAC

**HMAC** = hash + secret key. It proves the data was **not changed** and came from someone with the **key**.

---

## 7. 🧂 Salting and Password Storage

**Never store passwords in plain text.** Store a **hash** instead.

### The problem with plain hashes

Attackers keep big lists of pre-made hashes called **rainbow tables**. Same password = same hash = instant crack.

### The fix: Salt

A **salt** is random data added to the password **before** hashing. Each user gets a different salt.

```
 Password "ali123"  + Salt "9fK2"  ──► Hash ──► a81c...   (User 1)
 Password "ali123"  + Salt "Xp7Q"  ──► Hash ──► 52de...   (User 2)
        Same password, different hashes 🎉
```

### Fast vs slow hashes for passwords

| Algorithm | Speed | Good for passwords? |
|-----------|-------|---------------------|
| MD5 / SHA-256 alone | Very fast | ❌ Too easy to brute-force |
| **bcrypt** | Slow (adjustable) | ✅ Yes |
| **scrypt** | Slow, memory heavy | ✅ Yes |
| **Argon2** | Slow, memory heavy | ✅ Best choice today |
| PBKDF2 | Slow (many rounds) | ✅ Yes |

> 🌶️ A **pepper** is a secret value added to all passwords, stored **outside** the database.

### Linux password hashes (`/etc/shadow`)

| Prefix | Algorithm |
|--------|-----------|
| `$1$` | MD5 |
| `$5$` | SHA-256 |
| `$6$` | SHA-512 |
| `$y$` | yescrypt |
| `$2a$` / `$2y$` | bcrypt |

---

## 8. ✍️ Digital Signatures

A **digital signature** proves **who sent** the data and that it was **not changed**.

**Analogy:** A **handwritten signature** ✍️ that is impossible to fake and also breaks if the paper is edited.

```
 SIGN (sender)                          VERIFY (receiver)
 Message ─► Hash ─► Encrypt hash        Message ─► Hash ─┐
                    with PRIVATE key                     ├─► Match? ✅ / ❌
                          │                              │
                       Signature ──► Decrypt with PUBLIC key ─┘
```

| | Encryption | Signing |
|---|-----------|---------|
| Uses | Receiver's **public** key to lock | Sender's **private** key to sign |
| Opens with | Receiver's **private** key | Sender's **public** key |
| Gives | Secrecy | Trust and integrity |

**Real examples:** software updates, Git commit signing, code signing, email (PGP).

---

## 9. 📜 Certificates and TLS

How do you know the public key really belongs to `bank.com`? With a **digital certificate**.

**Analogy:** A **passport** 🛂 issued by a trusted government (the **Certificate Authority**).

```
 Root CA 🏛️  (trusted by your browser)
    └── Intermediate CA
           └── Website certificate (bank.com + public key)
```

| Part of a certificate | Meaning |
|-----------------------|---------|
| Subject | Who it belongs to (domain) |
| Public key | Website's public key |
| Issuer | The CA that signed it |
| Validity | Start and end dates |
| Signature | CA's digital signature |

### HTTPS / TLS Handshake (simplified)

```
 💻 Browser                                   🖥️ Server
    │ ── 1. Hello, I support these ciphers ──► │
    │ ◄─ 2. Hello + Certificate (public key) ─ │
    │ ── 3. Check certificate with trusted CA  │
    │ ── 4. Agree on a shared secret key ────► │  (key exchange)
    │ ◄══ 5. Encrypted traffic (AES) ═════════► │
```

- 🔒 Padlock in browser = TLS is working and certificate is valid.
- ⚠️ It does **not** mean the site is safe, only that the connection is encrypted.

---

## 10. 🤝 Key Exchange

**The problem:** Two people want a shared secret over an open network, and an attacker is listening.

### Diffie-Hellman (paint analogy) 🎨

```
 Alice + Bob agree on a public colour: YELLOW (everyone can see)
 Alice adds her secret RED   ─► sends ORANGE mix
 Bob   adds his secret BLUE  ─► sends GREEN mix
 Alice adds her RED to Bob's mix   ─┐
 Bob   adds his BLUE to Alice's mix ─┴─► Same final colour = shared secret 🤎
 An eavesdropper sees the mixes but cannot un-mix them.
```

| Type | Meaning |
|------|---------|
| DH / ECDH | Key exchange method |
| **Perfect Forward Secrecy (PFS)** | New key per session. If one key leaks, old sessions stay safe |

> ⚠️ Plain Diffie-Hellman has no identity check, so it can suffer a **man-in-the-middle** attack unless combined with certificates.

---

## 11. 🏛️ Classic Ciphers

These are old and easy to break, but great to learn and common in CTFs.

| Cipher | How it works | Example |
|--------|--------------|---------|
| **Caesar** | Shift each letter by N | A ➜ D (shift 3) |
| **ROT13** | Caesar with shift 13 | HELLO ➜ URYYB |
| **Vigenère** | Caesar with a repeating keyword | Uses a key like `KEY` |
| **Substitution** | Each letter replaced by another | A ➜ Q, B ➜ M ... |
| **XOR** | Combine bits with a key | `1010 ⊕ 1100 = 0110` |
| **Atbash** | Reverse alphabet | A ➜ Z, B ➜ Y |

```bash
echo "HELLO" | tr 'A-Za-z' 'N-ZA-Mn-za-m'    # ROT13 → URYYB
```

> 🔎 **Frequency analysis:** In English, `E` and `T` are the most common letters. Attackers count letters to break simple ciphers.

---

## 12. 💥 Common Attacks

| Attack | What it does | Defence |
|--------|--------------|---------|
| **Brute force** | Tries every possible key or password | Long keys, slow hashes |
| **Dictionary attack** | Tries common words and passwords | Strong, unique passwords |
| **Rainbow table** | Uses pre-computed hashes | Salting |
| **Collision attack** | Finds 2 inputs with the same hash | Use SHA-256 or better |
| **Man-in-the-middle** | Sits between two parties | Certificates, TLS |
| **Downgrade attack** | Forces weaker encryption | Disable old protocols |
| **Padding oracle** | Uses error messages to decrypt data | Use AES-GCM |
| **Side-channel** | Uses timing or power info | Constant-time code |
| **Key reuse / weak IV** | Repeats keys or IVs | Random, unique IVs |

> 🛡️ **Golden rule:** Never invent your own cryptography. Use well-tested libraries.

---

## 13. 🛠️ Useful Tools and Commands

| Goal | Tool | Command |
|------|------|---------|
| Base64 encode / decode | base64 | `echo "hi" \| base64` |
| Hash a string | sha256sum | `echo -n "hi" \| sha256sum` |
| Identify a hash type | hash-identifier | `hash-identifier` |
| Crack hashes | John the Ripper | `john --wordlist=rockyou.txt hash.txt` |
| Crack hashes (GPU) | Hashcat | `hashcat -m 0 hash.txt rockyou.txt` |
| Encrypt a file (AES) | openssl | `openssl enc -aes-256-cbc -salt -pbkdf2 -in a.txt -out a.enc` |
| Decrypt a file | openssl | `openssl enc -d -aes-256-cbc -pbkdf2 -in a.enc -out a.txt` |
| Make RSA private key | openssl | `openssl genrsa -out private.pem 2048` |
| Get public key | openssl | `openssl rsa -in private.pem -pubout -out public.pem` |
| Check a certificate | openssl | `openssl s_client -connect site.com:443` |
| Encrypt with GPG | gpg | `gpg -c file.txt` |
| Decode ciphers (CTF) | CyberChef | Web tool: "Magic" recipe |

### Hashcat mode quick list

| Hash | Mode |
|------|------|
| MD5 | `-m 0` |
| SHA-1 | `-m 100` |
| SHA-256 | `-m 1400` |
| SHA-512 | `-m 1700` |
| bcrypt | `-m 3200` |
| SHA-512 crypt (`$6$`) | `-m 1800` |

📸 **Screenshot:** _Hash cracking or OpenSSL output_
`![Crypto Tools](screenshots/crypto-tools.png)`

> ⚠️ Only crack hashes and test systems that you **own or have permission** to test.

---

## 14. 🧾 Cheat Sheet

| Term | One-line meaning |
|------|------------------|
| Encoding | Change format, no secret (Base64) |
| Encryption | Hide data with a key (AES, RSA) |
| Hashing | One-way fingerprint (SHA-256) |
| Symmetric | 1 shared key, fast |
| Asymmetric | Public + private key, slower |
| Salt | Random data to make hashes unique |
| HMAC | Hash with a secret key |
| Digital signature | Proves sender and integrity |
| Certificate | ID card for a public key |
| CA | Trusted issuer of certificates |
| TLS | Protocol that secures HTTPS |
| PFS | New key per session |
| IV / Nonce | Random start value, never reuse |

### Which algorithm should I use?

| Need | Use |
|------|-----|
| Encrypt data | AES-256-GCM or ChaCha20-Poly1305 |
| Share a key | ECDH / RSA (2048+) |
| Sign data | RSA-PSS / ECDSA / Ed25519 |
| Hash a file | SHA-256 or SHA-3 |
| Store passwords | Argon2, bcrypt, or scrypt |

---

## 15. 💡 Key Takeaways

- ✅ Encoding is not encryption. Base64 hides nothing.
- ✅ Symmetric is fast, asymmetric solves key sharing. HTTPS uses both.
- ✅ Hashes are one-way. Use **salt** and a **slow hash** for passwords.
- ✅ MD5 and SHA-1 are broken. Use SHA-256 or better.
- ✅ Digital signatures give trust. Certificates prove who owns a public key.
- ✅ Never reuse IVs or keys, and never make your own cipher.
- ✅ In CTFs, try CyberChef, `hash-identifier`, John, and Hashcat first.

---

⭐ _Notes written while learning cryptography for cybersecurity. Practice every command on your own files and lab machines._
