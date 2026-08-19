# Module 3.1: HTTP Request Anatomy 🔍

Understanding raw HTTP request structure allows an engineer to craft custom payloads, identify header parsing vulnerabilities, and bypass web application firewalls (WAFs).

---

## Part 1 — Big Picture 🌐

### The Problem

When a web browser sends user data (like a username or search term) to a remote web server, it needs a strict syntax so the web server's parser knows:

1. What action the client wants to take.


2. Which file or API endpoint is targeted.


3. What configuration options apply to the client.


4. Where the configuration ends and where the user data begins.



### What Happens Without Strict Syntax?

Without a standardized request structure, the web server's parser cannot distinguish between server configuration parameters and raw user input. A corrupted syntax leads to dropped requests, parser crashes, or memory corruption vulnerabilities.

---

## Part 2 — Mental Model ✉️

Think of an HTTP request like a physical business letter placed in an envelope:

```
+───────────────────────────────────────────────────────────────────+
| 1. THE ACTION STAMP (Request Line)                                |
|    "DELIVER THIS TO /login USING STANDARD 1.1"                    |[cite: 2]
+───────────────────────────────────────────────────────────────────+
| 2. ENVELOPE METADATA (Headers)                                    |
|    Destination Office: target.com                                 |[cite: 2]
|    Sender Device Type: Linux/Firefox                              |[cite: 2]
|    Security Clearance: Stamped Token #abc123                      |[cite: 2]
+───────────────────────────────────────────────────────────────────+
| 3. ENVELOPE SEAL (Mandatory Blank Line - CRLF)                    |
|    ====================== [EMPTY SPACE] ========================= |[cite: 2]
+───────────────────────────────────────────────────────────────────+
| 4. THE ACTUAL LETTER (Message Body)                               |
|    username=harish&password=secretPassword123                     |[cite: 2]
+───────────────────────────────────────────────────────────────────+

```

---

## Part 3 — Internal Working & The 4 Core Components ⚙️

An HTTP request consists of four distinct sections sent in sequential order:

```
                     HTTP REQUEST STRUCTURE
                               │
       ┌───────────────────────┼───────────────────────┬───────────────────────┐
       ▼                       ▼                       ▼                       ▼
1. Request Line         2. Request Headers      3. Blank Line           4. Request Body[cite: 2]
  - Method (Action)       - Key: Value pairs      - \r\n (CRLF)           - Raw Data Payload[cite: 2]
  - Path (Resource)       - Metadata & Context    - Strict Boundary       - POST / PUT data[cite: 2]
  - Protocol Version      - Target Host           - Parser delimiter      - Form/JSON/Binary[cite: 2]

```

### 1. The Request Line

The first line of every HTTP request. It contains three tokens separated by single spaces:


$$\text{[METHOD]} \quad \text{[PATH / URI]} \quad \text{[HTTP VERSION]}$$

```http
POST /api/v1/user/update HTTP/1.1

```

* **Method (`POST`):** The operation to execute (`GET`, `POST`, `PUT`, `DELETE`, `HEAD`, `OPTIONS`).


* **Path (`/api/v1/user/update`):** The exact resource path on the web server.


* **Protocol Version (`HTTP/1.1`):** Specifies the HTTP dialect the client expects.



### 2. Request Headers (Key-Value Pairs)

Metadata lines formatted as `Header-Name: Value`:

* **`Host: target.com`:** Mandatory in HTTP/1.1; indicates which virtual host should process the request on a shared server.


* **`User-Agent: Mozilla/5.0...`:** Identifies the client software, OS, and browser engine.


* **`Content-Type: application/x-www-form-urlencoded`:** Defines the MIME type format of the data contained in the body.


* **`Content-Length: 32`:** Informs the server parser of the exact byte count of the body.



### 3. The Mandatory Blank Line (`\r\n\r\n`)

In the ASCII standard, every line terminates with a Carriage Return (`\r` / `0x0D`) and Line Feed (`\n` / `0x0A`).

* To mark the end of the headers, the client sends a blank line: `\r\n`.


* This creates a byte sequence of `\r\n\r\n` (CRLF CRLF), telling the web server's parser: *"All metadata is complete; the next byte belongs to the message payload."*


### 4. Request Body

The raw data payload transmitted to the server.

* A `GET` or `HEAD` request typically has **no body**.


* A `POST`, `PUT`, or `PATCH` request places form variables, JSON structures, XML documents, or binary file data here.



---

## Part 4 — Real Packet Flow & Wire Format 📡

```
[ Client Application (e.g., curl / Browser) ]
                      │
                      ▼
[ Assembles Plain ASCII Buffer in Memory ]
  Line 1: "POST /login HTTP/1.1\r\n"
  Line 2: "Host: target.com\r\n"
  Line 3: "Content-Type: application/x-www-form-urlencoded\r\n"
  Line 4: "Content-Length: 29\r\n"
  Line 5: "\r\n"  <--- Empty Line Delimiter
  Line 6: "user=harish&pass=password123"
                      │
                      ▼
[ Writes byte buffer to TCP Socket Descriptor ]
                      │
                      ▼
[ Packets travel across Network Wire ]

```

```
Hexadecimal Byte Representation on Wire:
50 4f 53 54 20 2f 6c 6f 67 69 6e 20 48 54 54 50  | POST /login HTTP
2f 31 2e 31 0d 0a 48 6f 73 74 3a 20 74 61 72 67  | /1.1..Host: targ
65 74 2e 63 6f 6d 0d 0a 0d 0a 75 73 65 72 3d 68  | et.com....user=h
61 72 69 73 68 26 70 61 73 73 3d 70 61 73 73 31  | arish&pass=pass1

```

Notice `0d 0a 0d 0a` directly before `user=harish`. That is the exact hexadecimal representation of `\r\n\r\n`!

---

## Part 5 — Linux View 🐧

In Linux, when an HTTP server like Nginx receives this data:

1. The kernel places incoming bytes into the TCP socket's receive buffer (`sk_buff`).
2. The Nginx worker process calls `read()` on the socket file descriptor to pull the string into its internal memory buffer.
3. The Nginx state machine scans the buffer byte-by-byte until it detects the byte pattern `\r\n\r\n`.


4. It parses the lines above that point as headers and reads the remaining bytes defined by the `Content-Length` header as the body.



---

## Part 6 — Practical Demo 🛠️

Let's use `nc` to construct a raw HTTP `POST` request with all four anatomical sections manually.

### Step 1: Open a TCP connection

```bash
nc httpbin.org 80

```

### Step 2: Paste or Type the Request

Type the following lines carefully, ensuring you press **Enter twice** after the `Content-Length` line:

```http
POST /post HTTP/1.1
Host: httpbin.org
User-Agent: ManualClient/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 27

username=harish&role=admin

```

### Step 3: Observe the Server Processing

`httpbin.org` will read the four parts and return a `200 OK` JSON response reflecting the exact body data sent:

```json
{
  "form": {
    "role": "admin",
    "username": "harish"
  }
}

```

---

## Part 7 — Output Breakdown 🔍

| Section from Demo | Internal Component | Purpose |
| --- | --- | --- |
| `POST /post HTTP/1.1` | **Request Line**<br> | Tells server to execute `POST` method on `/post` endpoint using HTTP 1.1.

 |
| `Host: httpbin.org` | **Host Header**<br> | Required so multi-tenant reverse proxies route to the right virtual server.

 |
| `Content-Length: 27` | **Payload Length**<br> | The server reads exactly 27 bytes following the blank line (`username=harish&role=admin`).

 |
| *Blank line* | **CRLF Delimiter**<br> | Signals the transition from request headers to body data.

 |
| `username=harish&role=admin` | **Request Body**<br> | The parameters received and processed by the backend logic.

 |

---

## Part 8 — Security Perspective (Red Team vs. SOC) ⚔️

### Red Team Operator Perspective 🎯

1. **HTTP Request Smuggling (HRS):** If a front-end reverse proxy uses the `Content-Length` header to find the end of a request, but the back-end uses `Transfer-Encoding: chunked`, an attacker can smuggle a hidden second request inside the body of the first.
2. **Verb Tampering:** Changing the method in the Request Line (e.g., changing `GET /admin` to `HEAD /admin` or `POST /admin`) can bypass misconfigured authorization rules in web filters.


3. **Payload Injection via Body:** Form fields and JSON data in the Request Body are the primary delivery vector for SQL Injection, Command Injection, and Server-Side Template Injection (SSTI).



### SOC Analyst Perspective 🛡️

1. **Content-Length Mismatch Alerts:** IDS/WAF rules flag requests where the declared `Content-Length` does not match the actual byte count of the body.


2. **Illegal Character Scanning in Request Lines:** Monitoring for path traversal characters (`../` or `..%2f`) inside the Request Line path to block Local File Inclusion (LFI) attempts.
3. **Non-Standard Verb Monitoring:** Alerting on rare or administrative HTTP methods (like `PUT`, `DELETE`, `DEBUG`, or `TRACE`) reaching production endpoints.



---

## Part 9 — Common Beginner Mistakes ❌

1. **Calculating `Content-Length` Incorrectly:**
* If you declare `Content-Length: 20` but your body contains 35 bytes, the server only reads the first 20 bytes and ignores or misinterprets the rest.
* If you declare `Content-Length: 40` but only send 20 bytes, the server hangs waiting for the remaining 20 bytes until it hits a `408 Request Timeout`.


2. **Missing the Blank Line:**
* Forgetting the blank line between headers and body causes the server parser to treat the first line of your body data as an invalid header, returning a `400 Bad Request`.




3. **Sending Body Data with `GET` Requests:**
* While technically possible in some server implementations, standard HTTP caches, proxies, and firewalls will strip or ignore request bodies sent with `GET` methods.





---

## Part 10 — Professional Usage 💼

* **Red Team Operator:** Manually edits raw HTTP request lines and headers inside Burp Suite Repeater to craft precise exploit payloads and bypass front-end security filters.


* **SOC Analyst:** Inspects full raw HTTP packet captures (PCAPs) to extract malicious payloads delivered via HTTP POST requests.


* **Penetration Tester:** Tests for business logic flaws by modifying parameters inside both the Request Line and Request Body simultaneously.



---

## Part 11 — One-Page Summary 📝

```
=================================================================================
SUBHEADING 3.1: HTTP REQUEST ANATOMY MASTER SHEET
=================================================================================

1. THE FOUR MANDATORY SECTIONS:
   [1. Request Line]  : METHOD + PATH + HTTP/VERSION[cite: 2]
   [2. Headers]       : Key: Value metadata lines[cite: 2]
   [3. Blank Line]    : \r\n\r\n (CRLF) - Separates metadata from payload[cite: 2]
   [4. Message Body]  : Payload bytes (JSON, form data, files)[cite: 2]

2. ESSENTIAL RULES:
   - Request Line elements are separated by single spaces[cite: 2].
   - Headers use "Header-Name: Value" format[cite: 2].
   - Content-Length must match the exact byte size of the Message Body[cite: 2].
   - GET requests have no body; POST/PUT requests carry data in the body[cite: 2].

3. COMMON ATTACK VECTORS:
   - Request Line -> Verb Tampering, Path Traversal, LFI[cite: 2].
   - Headers      -> Host Header Injection, Header Smuggling[cite: 2].
   - Blank Line   -> HTTP Desync / Request Smuggling.
   - Body         -> SQLi, Command Injection, XSS, Deserialization[cite: 2].
=================================================================================

```

---

## Part 12 — Reasoning & Thinking Quiz 🧠

Answer these scenarios to verify your understanding of request anatomy:

1. **If you send an HTTP `POST` request with the header `Content-Length: 10`, but the text in your Request Body is `username=administrator` (22 bytes), what will the backend server process as the parameter value?**
2. Why does an HTTP request fail with a `400 Bad Request` if you accidentally omit the space between the HTTP Method and the path (e.g., `GET/index.html HTTP/1.1`)?


3. If you remove the `Host:` header from a request sent to an HTTP/1.1 web server, what status code should the server return according to the HTTP specification?


4. Why is an empty line (`\r\n\r\n`) strictly required even if an HTTP request has no body (like a standard `GET` request)?


5. How does an interception proxy like Burp Suite allow a security engineer to modify the Request Line and Body before the packet reaches the target server?
