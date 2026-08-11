# HTTP & Web Fundamentals
> **Harish-0xRed** · Red Team Learning Path · Phase 0 — Foundation  
> Part of the journey: `Networking → Linux → HTTP/Web → Python → Phase 1`

---

## 📍 Current Learning Status

```
✅ Cisco Networking Basics       — COMPLETED
✅ Linux Essentials (theory)     — COMPLETED
🔄 Linux Hands-on (Bandit)      — IN PROGRESS
🔄 HTTP/Web Fundamentals        — IN PROGRESS  ← YOU ARE HERE
⬜ Wireshark
⬜ Python for Hackers
⬜ Linux File Permissions & SUID
⬜ Bash Scripting
⬜ PowerShell + Web Basics
──────────────────────────────────
⬜ Phase 0 Complete → Start Phase 1
```

---

## Why HTTP Before Python?

Before automating something with Python, you must understand what you're automating.

```python
# This code means NOTHING if you don't know HTTP
import requests
r = requests.get("https://target.com/login")
print(r.status_code)
print(r.headers)
```

After finishing these 9 sections — every line above will make complete sense. That's the goal.

---

## Section 1 — How the Internet Works

> Foundation for everything. You know TCP/IP from Cisco — this deepens it.

### Concepts

| Topic | What It Means | Red Team Relevance |
|---|---|---|
| IP Address | Every device has a unique address on the network | Targets have IPs. You find them in recon. |
| DNS | Converts `google.com` → `142.250.x.x` | DNS enumeration finds subdomains of targets |
| TCP/IP | The road all internet traffic travels on | Every attack goes through TCP |
| Ports | Doors on a server — each service has one | Port 80=HTTP, 443=HTTPS, 22=SSH, 3389=RDP |
| Client-Server | You (client) request → server responds | You are the client. Target is the server. |

### How a Web Request Actually Works (Step by Step)

```
You type: https://google.com

Step 1  → Your machine asks DNS: "What is the IP of google.com?"
Step 2  → DNS answers: "142.250.195.46"
Step 3  → Your machine opens TCP connection to 142.250.195.46 on port 443
Step 4  → TLS handshake happens (encryption keys exchanged)
Step 5  → Your browser sends HTTP GET request inside encrypted tunnel
Step 6  → Google's server sends back HTML response
Step 7  → Browser renders the HTML into a page you see
```

### Direct Links

| What to Read | Link |
|---|---|
| What is an IP Address | `cloudflare.com/learning/network-layer/what-is-an-ip-address/` |
| What is DNS | `cloudflare.com/learning/dns/what-is-dns/` |
| What is TCP/IP | `cloudflare.com/learning/ddos/glossary/tcp-ip/` |
| What is a Port | `cloudflare.com/learning/network-layer/what-is-a-computer-port/` |
| How the Web Works (MDN) | `developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/How_the_Web_works` |

---

## Section 2 — HTTP Basics

> HTTP is the language browsers and servers use to talk. You MUST be fluent in this.

### Concepts

| Topic | What It Means |
|---|---|
| HTTP | HyperText Transfer Protocol — the language of the web |
| HTTP vs HTTPS | HTTP = plaintext (anyone can read it). HTTPS = encrypted with TLS |
| Request-Response | Every single web action = one request from you + one response from server |
| Stateless protocol | Server remembers NOTHING between requests. Cookies solve this. |

### The nc Proof — HTTP is Just Text

You already know nc. Here's proof HTTP is just readable text:

```bash
# Connect raw to google's HTTP server
nc google.com 80

# Type this manually and press Enter twice:
GET / HTTP/1.1
Host: google.com

# Google sends back raw HTML — that IS HTTP
```

HTTPS is the same thing but encrypted inside TLS — which is why you used `openssl s_client` in Bandit instead of nc.

### Direct Links

| What to Read | Link |
|---|---|
| HTTP Overview | `developer.mozilla.org/en-US/docs/Web/HTTP/Overview` |
| Why HTTP is not secure | `cloudflare.com/learning/ssl/why-is-http-not-secure/` |
| What is HTTPS | `cloudflare.com/learning/ssl/what-is-https/` |

---

## Section 3 — HTTP Requests (Critical)

> This is the most important section. Learn to read a raw HTTP request like reading English.

### Request Anatomy

```
GET /login HTTP/1.1
Host: target.com
User-Agent: Mozilla/5.0
Accept: text/html
Cookie: session=abc123
                             ← blank line separates headers from body
username=harish&password=123
```

| Part | What It Is |
|---|---|
| `GET` | The HTTP method (action being taken) |
| `/login` | The path on the server being requested |
| `HTTP/1.1` | The HTTP version being used |
| `Host: target.com` | Which website on the server (important — one server hosts many sites) |
| Headers | Extra metadata sent with every request |
| Blank line | Separator between headers and body |
| Body | Data sent to server — only exists in POST/PUT requests |

### URL Structure

```
https://target.com:443/search?query=harish&page=2#results
  │         │      │     │         │                │
  │         │      │     │         │                └── Fragment (anchor on page)
  │         │      │     │         └── Query parameters (data in URL)
  │         │      │     └── Path (resource on server)
  │         │      └── Port (443 is default for HTTPS)
  │         └── Domain name
  └── Protocol (scheme)
```

### Direct Links

| What to Read | Link |
|---|---|
| HTTP Messages (full breakdown) | `developer.mozilla.org/en-US/docs/Web/HTTP/Messages` |
| What is a URL | `developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL` |
| THM HTTP in Detail (Free Room) | `tryhackme.com/room/httpindetail` |

---

## Section 4 — HTTP Methods

> Every web action uses a method. Each method tells the server WHAT you want to do.

### Methods Table

| Method | What It Does | Red Team Use |
|---|---|---|
| `GET` | Fetch data from server | Parameters in URL — easy to manipulate |
| `POST` | Send data to server | Login forms, file uploads, API calls |
| `PUT` | Replace existing data on server | API abuse, unauthorized data replacement |
| `PATCH` | Partially update existing data | API abuse |
| `DELETE` | Remove data from server | Unauthorized deletion if auth is broken |
| `HEAD` | Get headers only — no body | Server fingerprinting without downloading content |
| `OPTIONS` | Ask server what methods it allows | Recon — reveals what the server can do |

### Red Team Note

```
GET  /user?id=1        → change to id=2 → access another user's data = IDOR vulnerability
POST /login            → body contains credentials → target for brute force
DELETE /api/user/5     → if no auth check → delete any user = broken access control
OPTIONS /              → reveals: Allow: GET, POST, PUT, DELETE → more attack surface
```

### Direct Links

| What to Read | Link |
|---|---|
| All HTTP Methods | `developer.mozilla.org/en-US/docs/Web/HTTP/Methods` |
| GET method | `developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET` |
| POST method | `developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST` |

---

## Section 5 — HTTP Status Codes

> Status codes are the server's response. Learn to read them instantly.

### Status Code Ranges

| Range | Meaning | What Server Is Saying |
|---|---|---|
| `1xx` | Informational | "Processing, hold on..." |
| `2xx` | Success | "Got it, here's your answer" |
| `3xx` | Redirect | "What you want is somewhere else" |
| `4xx` | Client Error | "You did something wrong" |
| `5xx` | Server Error | "I broke, not your fault" |

### Most Important Status Codes

| Code | Name | Red Team Meaning |
|---|---|---|
| `200` | OK | Request worked — normal response |
| `201` | Created | Something was created — check what |
| `301` | Permanent Redirect | Server moved — follow the redirect |
| `302` | Temporary Redirect | Can be used for open redirect attacks |
| `400` | Bad Request | Malformed request — useful in fuzzing |
| `401` | Unauthorized | Authentication required — login wall |
| `403` | Forbidden | **EXISTS but you're blocked** — try bypass |
| `404` | Not Found | Does not exist |
| `405` | Method Not Allowed | Try different HTTP method |
| `500` | Internal Server Error | **You broke something in the backend** — goldmine |
| `502` | Bad Gateway | Backend is down |

### Critical Red Team Difference

```
403 Forbidden  → The resource EXISTS but you don't have permission
               → Try: different User-Agent, IP, method, path variation
               
404 Not Found  → The resource DOES NOT EXIST
               → Stop trying this exact path, move on
               
500 Server Error → Your request caused a crash in the backend
               → This often means SQL injection or code injection point
```

### Direct Links

| What to Read | Link |
|---|---|
| All Status Codes | `developer.mozilla.org/en-US/docs/Web/HTTP/Status` |
| 4xx Client Errors | `developer.mozilla.org/en-US/docs/Web/HTTP/Status#client_error_responses` |
| 5xx Server Errors | `developer.mozilla.org/en-US/docs/Web/HTTP/Status#server_error_responses` |

---

## Section 6 — HTTP Headers

> The most important section for red teaming. Headers carry the metadata of every request and response. This is where attacks begin.

### Request Headers (You Send These)

| Header | What It Does | Attack Angle |
|---|---|---|
| `Host` | Tells server which site you want | Host header injection attacks |
| `User-Agent` | Identifies your browser/client | Spoof to bypass WAFs, access different content |
| `Cookie` | Sends stored cookies to server | **Session hijacking — steal this = steal account** |
| `Authorization` | Sends auth tokens (Bearer, Basic) | Credential theft, JWT attacks |
| `Content-Type` | Format of the request body | Content-type confusion, file upload bypass |
| `Referer` | Previous page URL | Information leakage about internal pages |
| `X-Forwarded-For` | Original IP (used by proxies) | IP spoofing to bypass IP-based restrictions |
| `Accept-Language` | Preferred language | Can change server behaviour |

### Response Headers (Server Sends These)

| Header | What It Does | Attack Angle |
|---|---|---|
| `Set-Cookie` | Server sets a cookie in your browser | **If no HttpOnly flag → steal via XSS** |
| `Location` | Where to redirect | Open redirect attacks |
| `Server` | Web server software and version | Fingerprinting → find known CVEs |
| `X-Powered-By` | Backend framework and version | Fingerprinting → Laravel/PHP/Express tells you stack |
| `Content-Security-Policy` | Prevents XSS attacks | **If MISSING → XSS is possible** |
| `Access-Control-Allow-Origin` | CORS policy | CORS misconfiguration attacks |
| `Strict-Transport-Security` | Forces HTTPS | If missing → SSL stripping possible |

### Example — Reading a Real Response Header

```
HTTP/1.1 200 OK
Server: Apache/2.4.41                ← Apache version → search CVEs for this version
X-Powered-By: PHP/7.2.0             ← PHP 7.2 → old version, many known CVEs
Set-Cookie: session=abc123           ← No HttpOnly flag! → vulnerable to XSS theft
Content-Security-Policy: (missing)  ← No CSP → XSS possible
```

In 4 lines you know: server software, PHP version, cookie theft possible, XSS possible. That's recon without touching a single attack tool.

### Direct Links

| What to Read | Link |
|---|---|
| All Headers Reference | `developer.mozilla.org/en-US/docs/Web/HTTP/Headers` |
| Cookie Header | `developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cookie` |
| User-Agent Header | `developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent` |
| Authorization Header | `developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization` |
| Content-Type Header | `developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type` |

---

## Section 7 — Cookies and Sessions

> Cookies are the most attacked component of web apps. Understand them at a deep level.

### What a Cookie Is

```
Server sends:   Set-Cookie: session=a1b2c3d4; HttpOnly; Secure; SameSite=Lax
Browser stores: session=a1b2c3d4
Next request:   Cookie: session=a1b2c3d4   ← sent automatically on every request
```

### Cookie Flags — Security Properties

| Flag | What It Does | If MISSING — Attack Possible |
|---|---|---|
| `HttpOnly` | JavaScript cannot read this cookie | ❌ XSS can steal the cookie |
| `Secure` | Cookie only sent over HTTPS | ❌ Cookie sent over HTTP too — sniffable |
| `SameSite=Lax` | Limits cross-site cookie sending | ❌ CSRF attack possible |
| `SameSite=Strict` | Cookie never sent cross-site | ✅ Strongest CSRF protection |

### Session vs Cookie

```
Cookie  → Stored in your browser. Small text. Sent on every request.
Session → Stored on the SERVER. Contains your actual login state.

Connection:
└── Cookie holds the SESSION ID (like a key)
└── Server uses that key to look up your session (the actual data)

Attack:
└── Steal the cookie → you have the key → you ARE that user to the server
└── No password needed — cookie IS the authentication
```

### JWT Tokens

```
Format: header.payload.signature
Example: eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoiaGFyaXNoIn0.SIGNATURE

Decode the middle part (payload) with base64:
echo "eyJ1c2VyIjoiaGFyaXNoIn0=" | base64 -d
→ {"user":"harish"}

Attack: Change "user":"harish" to "user":"admin", re-encode, submit
→ If server doesn't verify signature properly → you're admin
```

### Direct Links

| What to Read | Link |
|---|---|
| HTTP Cookies (full guide) | `developer.mozilla.org/en-US/docs/Web/HTTP/Cookies` |
| What are Cookies | `cloudflare.com/learning/privacy/what-are-cookies/` |

---

## Section 8 — HTTPS and TLS

> You already understand this from Bandit. Now see the full picture.

### The Problem with HTTP

```bash
# Run nc on port 80 and you see EVERYTHING in plaintext:
nc target.com 80
GET /login HTTP/1.1
Host: target.com
Cookie: session=abc123       ← anyone sniffing network can read this
                             ← username and password visible in body
```

### What TLS Adds

```
HTTP (plaintext):
You ──── username:harish, password:abc ────→ Server
         ↑ anyone on network can read this

HTTPS (TLS encrypted):
You ──── xK8#@!9mN2pL... ────→ Server
         ↑ encrypted — only you and server can read
         ↑ this is what openssl s_client does in Bandit
```

### TLS Handshake (Simplified)

```
Step 1 → Client says: "Hello, I want to connect securely"
Step 2 → Server sends: Its SSL certificate (proves identity)
Step 3 → Client verifies: Certificate is valid and trusted
Step 4 → Both agree on: Encryption algorithm and session keys
Step 5 → Encrypted tunnel established
Step 6 → HTTP request flows inside the tunnel
```

### The Big Misconception

```
❌ WRONG: "HTTPS means the website is safe"
✅ RIGHT: "HTTPS means the CONNECTION is encrypted"

Phishing sites use HTTPS too.
Malware sites use HTTPS too.
HTTPS only means: nobody between you and the server can read the traffic.
It says NOTHING about whether the server itself is trustworthy.
```

### Bandit Connection

```bash
# Level 14 — plaintext, used nc
nc 127.0.0.1 30000

# Level 15 — TLS encrypted, needed openssl
openssl s_client -connect 127.0.0.1:30001

# The reason: port 30001 requires TLS handshake first
# nc cannot do TLS → openssl handles it automatically
```

### Direct Links

| What to Read | Link |
|---|---|
| What is TLS | `cloudflare.com/learning/ssl/transport-layer-security-tls/` |
| What is an SSL Certificate | `cloudflare.com/learning/ssl/what-is-an-ssl-certificate/` |
| TLS Handshake Explained | `cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/` |

---

## Section 9 — How Websites Work

> Understand what you're attacking before you attack it.

### The Full Stack

```
                    [ Browser ]
                        │  HTTP request
                        ▼
                   [ Web Server ]         ← Nginx / Apache
                   (Nginx/Apache)         ← Receives HTTP, routes to app
                        │
                        ▼
                  [ Application ]         ← Python/PHP/Node/Java
                  (Backend Code)          ← Business logic lives here
                        │
                        ▼
                   [ Database ]           ← MySQL / PostgreSQL / MongoDB
                   (Data Store)           ← All user data, passwords, etc.
```

### Frontend vs Backend

| Layer | What It Is | Where It Runs | Attack Types |
|---|---|---|---|
| Frontend | HTML + CSS + JavaScript | Your browser | XSS, client-side injection |
| Backend | Python / PHP / Node / Java | Server | SQLi, RCE, SSRF, LFI |
| Database | MySQL / MongoDB | Server | SQL Injection |
| Web Server | Nginx / Apache | Server | Misconfig, path traversal |

### APIs

```
Old websites:  Server sends back HTML → browser renders it visually
Modern apps:   Server sends back JSON → JavaScript displays it

API example:
GET /api/user/1
→ Response: {"id":1,"username":"harish","role":"user","email":"h@h.com"}

Attack: Change /api/user/1 to /api/user/2 → access another user's data = IDOR
```

### The Attack Surface

```
Every layer is a potential entry point:

Frontend      → XSS, clickjacking, prototype pollution
HTTP Headers  → Host injection, CORS abuse
Web Server    → Directory traversal, misconfig
Backend Code  → SQLi, SSRF, RCE, command injection
Database      → SQL injection, NoSQL injection
Auth System   → Broken auth, JWT attacks, session fixation
```

### Direct Links — Free (Replaces Paid THM Room)

| What to Read | Link |
|---|---|
| How the Web Works (MDN) | `developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/How_the_Web_works` |
| Frontend vs Backend (MDN) | `developer.mozilla.org/en-US/docs/Learn/Server-side/First_steps/Introduction` |
| What is a Web Server | `developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_web_server` |
| The Odin Project — How Web Works | `theodinproject.com/lessons/foundations-how-does-the-web-work` |

---

## Complete Free Resource Map

| Topic | Direct Link | Free? |
|---|---|---|
| HTTP Full Reference | `developer.mozilla.org/en-US/docs/Web/HTTP` | ✅ |
| IP Addresses | `cloudflare.com/learning/network-layer/what-is-an-ip-address/` | ✅ |
| DNS Explained | `cloudflare.com/learning/dns/what-is-dns/` | ✅ |
| TCP/IP | `cloudflare.com/learning/ddos/glossary/tcp-ip/` | ✅ |
| What is a Port | `cloudflare.com/learning/network-layer/what-is-a-computer-port/` | ✅ |
| HTTP Overview | `developer.mozilla.org/en-US/docs/Web/HTTP/Overview` | ✅ |
| HTTP Messages | `developer.mozilla.org/en-US/docs/Web/HTTP/Messages` | ✅ |
| HTTP Methods | `developer.mozilla.org/en-US/docs/Web/HTTP/Methods` | ✅ |
| HTTP Status Codes | `developer.mozilla.org/en-US/docs/Web/HTTP/Status` | ✅ |
| HTTP Headers | `developer.mozilla.org/en-US/docs/Web/HTTP/Headers` | ✅ |
| HTTP Cookies | `developer.mozilla.org/en-US/docs/Web/HTTP/Cookies` | ✅ |
| HTTPS | `cloudflare.com/learning/ssl/what-is-https/` | ✅ |
| TLS Handshake | `cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/` | ✅ |
| THM: DNS in Detail | `tryhackme.com/room/dnsindetail` | ✅ |
| THM: HTTP in Detail | `tryhackme.com/room/httpindetail` | ✅ |
| THM: Putting It Together | `tryhackme.com/room/puttingitalltogether` | ✅ |
| PortSwigger Getting Started | `portswigger.net/web-security/getting-started` | ✅ |

---

## 7-Day Study Plan (30 min/day alongside Bandit)

```
Day 1 → Section 1: Cloudflare — IP + DNS + TCP + Ports articles
Day 2 → Section 2 + 3: MDN HTTP Overview + HTTP Messages
Day 3 → Section 4 + 5: MDN Methods + Status Codes
        + THM: DNS in Detail room
Day 4 → Section 6: MDN Headers (all of them)
        + THM: HTTP in Detail room
Day 5 → Section 7: MDN Cookies full guide
        + Cloudflare HTTPS + TLS articles
Day 6 → Section 8 + 9: TLS Handshake + MDN How Web Works
        + Odin Project web lesson
Day 7 → THM: Putting It All Together room
        + PortSwigger: Getting Started page
────────────────────────────────────────────
After Day 7 → HTTP/Web Fundamentals COMPLETE ✅
Next target  → Wireshark (Week 3)
```

---

## Key Takeaways for Red Teaming

```
1. HTTP is just text — nc proved that in Bandit
2. HTTPS = same text but inside TLS encryption — openssl handles that
3. Every web attack goes through HTTP — learn to READ requests fluently
4. Headers carry the attack surface — Server version, missing flags, cookies
5. 403 ≠ 404 — exists-but-blocked vs doesn't-exist. Critical difference.
6. Steal cookie = steal session = no password needed
7. 500 error = you broke the backend = potential injection point
8. HTTPS doesn't mean safe — phishing sites use HTTPS too
9. Backend runs your actual attack payload — that's where SQLi, RCE live
10. Every layer of the stack = its own attack surface
```

---

*Harish-0xRed · Phase 0 Foundation Notes · Red Team Learning Path*  
*Next: Wireshark → Python → Phase 1 (Nmap, OSINT, Recon)*
