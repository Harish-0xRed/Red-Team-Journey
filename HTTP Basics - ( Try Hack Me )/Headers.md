# 📘 HTTP Headers - Learning Notes

**Date:** 08-07-2026

---

# What are HTTP Headers?

HTTP Headers are **key-value pairs** that carry additional information (metadata) between the **client** and the **server** during an HTTP request and response.

They help both sides understand **how to process the data** being exchanged.

Example:

```http
GET / HTTP/1.1
Host: example.com
User-Agent: Chrome
```

---

# Why are HTTP Headers Important?

Headers provide important information such as:

- Which website is being requested
- Which browser is making the request
- Authentication information
- Session management
- Compression support
- Content type
- Caching rules

Without headers, modern websites would not function properly.

---

# Types of HTTP Headers

There are two main categories:

```
HTTP Headers
│
├── Request Headers
│      (Client → Server)
│
└── Response Headers
       (Server → Client)
```

---

# Request Headers

These headers are sent by the **client (browser)** to the **server**.

---

## Host

```http
Host: example.com
```

### Purpose

Identifies which website the client wants to access.

Useful because one web server can host multiple websites (Virtual Hosting).

---

## User-Agent

```http
User-Agent: Mozilla/5.0 Chrome
```

### Purpose

Identifies the browser or application making the request.

Examples:

- Chrome
- Firefox
- Edge
- Safari
- curl
- Python requests

**Security Note**

Attackers can spoof the User-Agent header.

---

## Content-Length

```http
Content-Length: 45
```

### Purpose

Specifies the size of the request body.

Mostly used with:

- POST
- PUT

---

## Accept-Encoding

```http
Accept-Encoding: gzip, br
```

### Purpose

Informs the server which compression methods the client supports.

Common compression algorithms:

- gzip
- br (Brotli)
- deflate

This helps reduce bandwidth and improve page loading speed.

---

## Cookie

```http
Cookie: session=ABC123
```

### Purpose

Sends stored session information to the server.

Common uses:

- Login sessions
- User preferences
- Shopping carts

---

# Response Headers

These headers are sent by the **server** to the **client**.

---

## Set-Cookie

```http
Set-Cookie: session=ABC123
```

### Purpose

Creates a cookie and instructs the browser to store it.

The browser automatically sends it back using the Cookie header in future requests.

---

## Cache-Control

```http
Cache-Control: max-age=3600
```

### Purpose

Controls how long the browser should cache the response.

Benefits:

- Faster loading
- Less bandwidth usage
- Reduced server load

---

## Content-Type

```http
Content-Type: text/html
```

### Purpose

Specifies the type of data returned by the server.

Common values:

| Content-Type | Description |
|--------------|-------------|
| text/html | HTML page |
| text/plain | Plain text |
| text/css | CSS |
| application/javascript | JavaScript |
| application/json | JSON |
| image/png | PNG Image |
| application/pdf | PDF Document |

---

## Content-Encoding

```http
Content-Encoding: gzip
```

### Purpose

Indicates how the response was compressed before being sent.

Common values:

- gzip
- br
- deflate

The browser automatically decompresses the content.

---

# Request vs Response Headers

| Request Headers | Purpose |
|-----------------|---------|
| Host | Target website |
| User-Agent | Browser or client information |
| Content-Length | Size of request body |
| Accept-Encoding | Supported compression methods |
| Cookie | Sends stored session information |

| Response Headers | Purpose |
|------------------|---------|
| Set-Cookie | Stores a cookie in the browser |
| Cache-Control | Controls browser caching |
| Content-Type | Type of returned content |
| Content-Encoding | Compression method used |

---

# HTTP Header Flow

```
Browser
    │
    │ Request Headers
    │
    ├── Host
    ├── User-Agent
    ├── Cookie
    └── Accept-Encoding
    │
    ▼
Web Server
    │
Processes Request
    │
    ▼
Response Headers
    │
    ├── Content-Type
    ├── Set-Cookie
    ├── Cache-Control
    └── Content-Encoding
    │
    ▼
Browser
```

---

# Real Example

### Request

```http
GET /profile HTTP/1.1
Host: example.com
User-Agent: Chrome
Accept-Encoding: gzip
Cookie: session=ABC123
```

### Response

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Encoding: gzip
Cache-Control: max-age=3600
Set-Cookie: theme=dark
```

---

# Cybersecurity Notes

Headers are frequently analyzed during:

- Web penetration testing
- API security testing
- Burp Suite interception
- Wireshark packet analysis
- SOC log investigations

Important headers for security:

- **Host** → Virtual host identification
- **User-Agent** → Client identification (can be spoofed)
- **Cookie** → Session management (target for session hijacking)
- **Content-Type** → Prevents incorrect content interpretation
- **Cache-Control** → Protects sensitive data from being cached

---

# Interview Points

**Q:** What are HTTP Headers?

> Metadata exchanged between the client and server that provides additional information about the request or response.

**Q:** Name two request headers.

- Host
- User-Agent

**Q:** Name two response headers.

- Content-Type
- Set-Cookie

**Q:** What is the difference between Cookie and Set-Cookie?

- **Set-Cookie** → Server instructs the browser to store a cookie.
- **Cookie** → Browser sends the stored cookie back to the server.

---

# Quick Revision

```
Request Headers
───────────────
Host
User-Agent
Content-Length
Accept-Encoding
Cookie

↓

Server

↓

Response Headers
────────────────
Set-Cookie
Content-Type
Cache-Control
Content-Encoding
```

---

# Key Takeaways

- HTTP Headers carry metadata between the client and server.
- Request headers describe the client and its request.
- Response headers describe the server's response.
- Cookies, content types, caching, and compression are managed using headers.
- Understanding headers is essential for web development, APIs, SOC analysis, and penetration testing.

---
