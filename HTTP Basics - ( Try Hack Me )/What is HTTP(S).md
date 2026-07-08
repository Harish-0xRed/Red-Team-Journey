# 📘 What is HTTP(S)? - Learning Notes

**Date:** 08-07-2026

---

# What is HTTP?

**HTTP (HyperText Transfer Protocol)** is the communication protocol used between a **client (browser)** and a **web server** to transfer web resources such as HTML pages, images, videos, JSON data, CSS, and JavaScript.

It defines **how a client requests data and how a server responds**.

---

# What is HTTPS?

**HTTPS (HyperText Transfer Protocol Secure)** is the secure version of HTTP.

It uses **TLS (Transport Layer Security)** to encrypt communication between the client and the server, protecting data from eavesdropping and tampering.

```
HTTP  = Communication
HTTPS = Secure Communication (HTTP + TLS Encryption)
```

---

# Why is HTTP Important?

HTTP is the foundation of the World Wide Web.

It is used for:

* Browsing websites
* Calling REST APIs
* Downloading files
* Uploading forms
* Mobile applications
* Cloud services

---

# Why HTTPS is Important?

HTTPS provides three major security benefits:

### 1. Encryption 🔒

Protects data from being read by attackers.

Example:

```
Username
Password
Credit Card
```

are encrypted during transmission.

---

### 2. Integrity ✅

Ensures data is not modified while travelling between the client and server.

---

### 3. Authentication 🆔

Confirms that you're communicating with the legitimate website using a **digital certificate**.

---

# HTTP vs HTTPS

| Feature              | HTTP                        | HTTPS                              |
| -------------------- | --------------------------- | ---------------------------------- |
| Full Form            | HyperText Transfer Protocol | HyperText Transfer Protocol Secure |
| Encryption           | ❌ No                        | ✅ Yes (TLS)                        |
| Default Port         | 80                          | 443                                |
| Security             | Low                         | High                               |
| Certificate Required | ❌ No                        | ✅ Yes                              |
| Recommended Today    | ❌ No                        | ✅ Yes                              |

---

# How HTTP Works

```
Browser
    │
    │ HTTP Request
    ▼
Web Server
    │
    │ Process Request
    ▼
HTTP Response
    │
    ▼
Browser Displays Website
```

Example:

```http
GET / HTTP/1.1
Host: example.com
```

Server:

```http
HTTP/1.1 200 OK
Content-Type: text/html

<html>...</html>
```

---

# How HTTPS Works

```
Browser
      │
      │ TLS Handshake
      ▼
Server

Certificate Verified

↓

Encrypted Connection Established

↓

HTTP Requests & Responses Sent Securely
```

The HTTP request is the same, but it travels through an encrypted TLS connection.

---

# Common HTTP Methods

* GET → Retrieve data
* POST → Create/Submit data
* PUT → Update existing data
* DELETE → Remove data

---

# Common HTTP Status Codes

| Code | Meaning               |
| ---- | --------------------- |
| 200  | OK                    |
| 201  | Created               |
| 301  | Moved Permanently     |
| 302  | Temporary Redirect    |
| 400  | Bad Request           |
| 401  | Unauthorized          |
| 403  | Forbidden             |
| 404  | Not Found             |
| 405  | Method Not Allowed    |
| 500  | Internal Server Error |
| 503  | Service Unavailable   |

---

# HTTP Communication Components

```
URL
        ↓
HTTP Request
        ↓
Headers
        ↓
Server
        ↓
HTTP Response
        ↓
Status Code
        ↓
Browser
```

---

# Cybersecurity Notes

* HTTP traffic is **unencrypted**, making it vulnerable to packet sniffing on insecure networks.
* HTTPS encrypts data using **TLS**, protecting credentials and sensitive information.
* Security analysts inspect HTTP requests and responses in tools like:

  * Burp Suite
  * Wireshark
  * Browser Developer Tools
  * curl
* Many web vulnerabilities (SQL Injection, XSS, CSRF, IDOR) are identified by analyzing HTTP traffic.

---

# Interview Points

**Q:** What does HTTP stand for?

> HyperText Transfer Protocol.

**Q:** What is the difference between HTTP and HTTPS?

> HTTPS is HTTP protected with TLS encryption, providing confidentiality, integrity, and server authentication.

**Q:** Which ports do they use?

* HTTP → **80**
* HTTPS → **443**

**Q:** Is HTTP stateful?

> No. HTTP is **stateless**. Cookies and sessions are used to maintain user state.

---

# Quick Revision

```
HTTP
↓

Client ↔ Server Communication

↓

No Encryption

↓

Port 80

----------------------

HTTPS

↓

HTTP + TLS

↓

Encrypted Communication

↓

Port 443

----------------------

HTTP is Stateless

↓

Cookies & Sessions provide memory
```

---

# Key Takeaways

* HTTP is the protocol used for web communication.
* HTTPS secures HTTP using TLS encryption.
* HTTP uses **port 80**, HTTPS uses **port 443**.
* HTTP is **stateless**, so cookies and sessions are needed to remember users.
* Understanding HTTP(S) is essential for networking, APIs, SOC analysis, penetration testing, and web application security.

---
