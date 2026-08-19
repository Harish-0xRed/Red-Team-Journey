# Section 3: HTTP Requests — Comprehensive Overview 🌐

Every web vulnerability, from SQL Injection to Broken Object-Level Authorization (IDOR), begins with altering a raw **HTTP Request**. As a Red Team operator, you must read, dissect, and manipulate raw request packets just like plain text.

---

### The Big Picture: What an HTTP Request Actually Is

An HTTP request is a formatted text message sent across a TCP/TLS socket from a client to a server. The server reads the message from top to bottom, parses the instructions, and decides how to process the request.

```
+─────────────────────────────────────────────────────────────────────────────+
|                             RAW HTTP REQUEST BLOCK                          |
+─────────────────────────────────────────────────────────────────────────────+
  GET /login HTTP/1.1                                <── 1. REQUEST LINE[cite: 2]
  Host: target.com                                   <── 2. HEADERS (Metadata)[cite: 2]
  User-Agent: Mozilla/5.0                            <──    (Key-Value pairs)[cite: 2]
  Accept: text/html                                  <──   [cite: 2]
  Cookie: session=abc123                             <──   [cite: 2]
                                                     <── 3. MANDATORY BLANK LINE[cite: 2]
  username=harish&password=123                       <── 4. REQUEST BODY (Payload)[cite: 2]
+─────────────────────────────────────────────────────────────────────────────+

```

---

### Architecture Breakdown of Section 3

Section 3 is divided into three core pillars that define how web resources are addressed and fetched:

```
                                SECTION 3 ROADMAP
                                        │
     ┌──────────────────────────────────┼──────────────────────────────────┐
     ▼                                  ▼                                  ▼
[ 1. Request Anatomy ]        [ 2. URL Architecture ]        [ 3. Data Passing Mechanisms ]
- Request Line (Method/Path)  - Protocol & Host Parsing      - URL Query Strings (GET)[cite: 2]
- Header Key-Value Pairs      - Port Targeting               - Request Body Payloads (POST)[cite: 2]
- The CRLF (\r\n\r\n) Boundary- Path vs. Query Parameters    - Encoding Types & Boundaries[cite: 2]
- Body Payload Structure      - Client-Side Fragments (#)[cite: 2]

```

---

### Core Components Summary

| Component | Function | Red Team / Offensive Relevance |
| --- | --- | --- |
| **Request Line**<br> | Defines the action (`GET`, `POST`), endpoint (`/login`), and protocol version (`HTTP/1.1`).

 | Method tampering (e.g., switching `GET` to `POST` to bypass basic ACLs), Path Traversal (`/../../etc/passwd`).

 |
| **Request Headers**<br> | Supplies context, client info, authentication state, and formatting rules.

 | Header injection, Host header spoofing, IP restriction bypasses (`X-Forwarded-For`), session hijacking.

 |
| **Blank Line (`\r\n\r\n`)**<br> | The strict delimiter informing the parser that metadata is complete.

 | HTTP Request Smuggling and HTTP Desync attacks. |
| **Request Body**<br> | Contains user-submitted form data, JSON payloads, or uploaded files.

 | Primary injection vector for SQLi, Command Injection, XSS, and authentication brute-forcing.

 |
| **URL Structure**<br> | Standardized locator mapping scheme, host, port, path, parameters, and fragments.

 | Parameter tampering, IDOR (`?id=1` ➔ `?id=2`), Open Redirects, and SSRF attacks.

 |

---

### URL vs. Request Line Relationship

When you type a full URL into a browser or pass it into `curl`, the client software automatically deconstructs the URL into an HTTP request before transmitting it:

```
URL Entered:
https://target.com:443/search?query=harish&page=2#results[cite: 2]
  │         │      │     │         │                │
  │         │      │     │         │                └── Fragment (Handled locally by browser only)[cite: 2]
  │         │      │     │         └─────────────────── Query Parameters (Sent in Request Line)[cite: 2]
  │         │      │     └───────────────────────────── Path (Resource on Server)[cite: 2]
  │         │      └─────────────────────────────────── Destination TCP Port[cite: 1, 2]
  │         └────────────────────────────────────────── Host Header[cite: 2]
  └──────────────────────────────────────────────────── Protocol Scheme (Triggers TLS wrapper)[cite: 2]

Deconstructed into Wire Transmission:
CONNECT target.com:443 (via TLS)[cite: 1, 2]
GET /search?query=harish&page=2 HTTP/1.1[cite: 2]
Host: target.com[cite: 2]

```

---

We will tackle Section 3 in three focused sub-modules:

1. **Subheading 3.1 — Request Anatomy** (Request Line, Headers, Blank Line, Body).


2. **Subheading 3.2 — URL Structure & Mechanics** (Scheme, Host, Port, Path, Query, Fragment).


3. **Subheading 3.3 — Data Passing & Parameter Manipulation** (Query Parameters vs. Request Body, Encoding, and IDORs).



Shall we begin with **Subheading 3.1 — Request Anatomy**?
