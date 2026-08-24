# Section 9 — How Websites Work

Before you can break an application, you must understand every layer of the technology stack supporting it. A modern web application is not a single program; it is an interconnected pipeline of services running across client and server environments.

---

**The Full Stack Architecture**

```
                     [ Browser (Client) ] 💻
                               │
                               │  HTTP / HTTPS Request[cite: 2]
                               ▼
                    [ Web Server (Reverse Proxy) ] 🌐   <── Nginx / Apache[cite: 2]
                    (Handles TLS, Static Files, Routing)[cite: 2]
                               │
                               │  Reverse Proxy / WSGI / FastCGI
                               ▼
                 [ Application Layer (Backend Code) ] ⚙️  <── Python / PHP / Node.js / Java[cite: 2]
                 (Business Logic, Authentication, API)[cite: 2]
                               │
                               │  SQL Queries / NoSQL Operations
                               ▼
                   [ Database (Data Store) ] 🗄️           <── MySQL / PostgreSQL / MongoDB[cite: 2]
                   (Stores User Records, Hashes, State)[cite: 2]

```

* **Client Browser:** Parses HTML, compiles CSS, executes JavaScript locally, and issues HTTP requests.


* **Web Server (Nginx / Apache):** Frontline listener on ports 80/443. Serves static files directly and forwards dynamic requests to the backend.


* **Application Backend (Python/PHP/Node):** Executes core application logic, checks authentication state, and computes dynamic responses.


* **Database (MySQL/MongoDB):** The persistent data storage holding accounts, secrets, and application records.



---

**Frontend vs. Backend Breakdown**

| Layer | What It Is | Where It Runs | Primary Attack Types |
| --- | --- | --- | --- |
| **Frontend**<br> | HTML, CSS, JavaScript

 | Client Browser

 | **Cross-Site Scripting (XSS)**, Client-Side Validation Bypass, Prototype Pollution

 |
| **Web Server**<br> | Nginx, Apache, IIS

 | Server OS

 | **Server Misconfiguration**, Path Traversal, Directory Listing, Known Server CVEs

 |
| **Backend Code**<br> | Python, PHP, Node.js, Java, Go

 | Server Runtime

 | **Remote Code Execution (RCE)**, SQLi, SSRF, Local File Inclusion (LFI), IDOR

 |
| **Database**<br> | MySQL, PostgreSQL, MongoDB

 | Database Server

 | **SQL Injection (SQLi)**, NoSQL Injection, Data Exfiltration

 |
| **Auth System**<br> | Cookies, Sessions, JWT, OAuth

 | Client & Server

 | **Broken Authentication**, Session Hijacking, JWT Signature Forgery

 |

---

**Modern APIs vs. Legacy Multi-Page Websites**

Web architectures have evolved from static document delivery to JSON-driven API models:

```
Traditional Architecture:
Client ───( GET /user.html )───► Server generates complete HTML page ───► Browser displays page[cite: 2]

Modern API Architecture:
Client ───( GET /api/user/1 )──► Server returns raw JSON payload: ──────► JavaScript renders data dynamically[cite: 2]
                                {"id":1, "username":"harish", "role":"user"}[cite: 2]

```

* **The Attack Angle:** Modern Single Page Applications (SPAs) expose structured REST and GraphQL endpoints. Modifying identifiers (e.g., requesting `/api/user/2`) exposes backend data via **Insecure Direct Object References (IDOR)** when access control checks are absent on the API layer.



---

**The Full-Stack Attack Surface**

An operator assesses each layer independently during an engagement:

```
Frontend        ──► XSS, Clickjacking, DOM Manipulation, Secrets in JS files[cite: 2]
HTTP Headers    ──► Host Header Injection, CORS Misconfigurations, Missing Security Flags[cite: 2]
Web Server      ──► Directory Traversal, Outdated Software Exploits, Misconfigured Aliases[cite: 2]
Backend Code    ──► SQLi, Server-Side Request Forgery (SSRF), Command Injection, RCE[cite: 2]
Database        ──► SQL / NoSQL Injection, Database Credential Theft[cite: 2]
Auth System     ──► Broken Access Control, Session Fixation, JWT Tampering[cite: 2]

```

---

**Reasoning & Architecture Quiz 🧠**

1. If a developer uses client-side JavaScript to disable the "Submit" button when a price is less than $10, why can a Red Team operator easily bypass this restriction using `curl` or Burp Suite?


2. In a stack with Nginx as a reverse proxy and Python Flask as the application backend, where does an `HTTP 502 Bad Gateway` error originate?


3. Why does finding database credentials in a compromised backend configuration file allow access to the database layer even if the database port is firewalled from the public internet?
