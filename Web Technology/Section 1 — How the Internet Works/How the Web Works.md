# Module: How the Web Works 🌐

Welcome to **How the Web Works**. In this module, we step up from raw transport ports and IP addresses to the entire architecture of the modern web.

---

## Part 1 — Big Picture 🌐

### Why it exists

The Web exists to allow computers anywhere in the world to request, serve, and render human-readable documents, media, and interactive applications over standardized network protocols.

### What problem it solves

Before the Web, sharing documents required custom software, specific file formats, and manual connections. The Web created a standard language (**HTTP/HTTPS**), standard document formats (**HTML/CSS/JS**), and a standard addressing system (**URLs**) so any client browser can interact with any web server.

### What happens if it didn't exist?

Without the Web:

* There are no web browsers, no web applications, no cloud portals, and no REST APIs.
* Every service would require its own standalone client application.
* For a Red Team operator: There would be no web attack surface (No XSS, SQLi, CSRF, IDOR, or web shell execution).

---

## Part 2 — Mental Model 🏬

Imagine ordering a customized meal from a restaurant:

```
[ Customer (Browser / Client) ] 👤
       │
       ▼ (1. Looks up address in Phonebook)
[ Phonebook (DNS) ] 📖 ───► Finds address: "100 Main St"
       │
       ▼ (2. Travels to restaurant & sits down)
[ Table Reserved (TCP/TLS Connection) ] 🤝
       │
       ▼ (3. Hands written order to waiter)
[ Order Slip (HTTP GET Request) ] 📜
       │
       ▼ (4. Kitchen prepares food)
[ Kitchen (Web Server + Backend Code + Database) ] 👨‍🍳
       │
       ▼ (5. Waiter brings back raw dishes)
[ Plated Food (HTTP Response: HTML / CSS / JS) ] 🍲
       │
       ▼ (6. Customer arranges dishes on table to eat)
[ Assembled Meal (Browser Rendered Page) ] 🍽️

```

* **Client (Browser):** The customer sitting at the table making orders.
* **Server:** The kitchen preparing and serving items.
* **DNS:** The phonebook translating names to street addresses.
* **HTTP Request:** The order slip listing what you want (`GET /menu.html`).
* **HTTP Response:** The platter holding the requested item along with the receipt status (`200 OK`).
* **Static Resource:** Pre-made items sitting in the display case (like canned soda or bread).
* **Dynamic Resource:** Meals cooked fresh to order based on customer input (like a custom hamburger with extra cheese).

---

## Part 3 — Internal Working & Full Architecture ⚙️

### 1. Key Components Defined

* **Client:** An application (Web Browser, `curl`, Python script) that initiates network requests to retrieve or submit web resources.
* **Server:** A remote system running web server software (Nginx, Apache, IIS) that listens for incoming HTTP requests and returns resources.
* **Static Resources:** Files stored directly on the server's disk that are delivered to the client without modification (HTML files, CSS stylesheets, JavaScript files, images like `.png`/`.jpg`).
* **Dynamic Resources:** Content generated on-the-fly by backend code (PHP, Python, Node.js, Java) before being sent back. (Example: Your custom user dashboard showing your profile picture and private messages).

---

### 2. Full URL Lifecycle Flowchart

```
                          User enters: https://target.com/profile
                                             │
                                             ▼
                             +───────────────────────────────+
                             | 1. DNS Resolution             |
                             | (Domain Name -> IP Address)   |
                             +───────────────────────────────+
                                             │
                                             ▼
                             +───────────────────────────────+
                             | 2. TCP 3-Way Handshake        |
                             | (Establish connection port 443)|
                             +───────────────────────────────+
                                             │
                                             ▼
                             +───────────────────────────────+
                             | 3. TLS Handshake              |
                             | (Negotiate Encryption Keys)   |
                             +───────────────────────────────+
                                             │
                                             ▼
                             +───────────────────────────────+
                             | 4. HTTP Request Sent          |
                             | (GET /profile HTTP/1.1)       |
                             +───────────────────────────────+
                                             │
                                             ▼
                             +───────────────────────────────+
                             | 5. Web Server Processing      |
                             | (Nginx/App Code/Database query)|
                             +───────────────────────────────+
                                             │
                                             ▼
                             +───────────────────────────────+
                             | 6. HTTP Response Sent         |
                             | (HTTP/1.1 200 OK + HTML body)  |
                             +───────────────────────────────+
                                             │
                                             ▼
                             +───────────────────────────────+
                             | 7. Browser Parsing & Render   |
                             | (DOM Construction & Fetching) |
                             +───────────────────────────────+

```

---

## Part 4 — Real Packet Flow & Asset Resolution 📡

When a browser renders a page, it does **not** download everything in one single request. It makes an initial request for HTML, parses it, and then generates **secondary requests** for linked files (CSS, JS, Images).

```
Client (Browser)                                                Server (Nginx/Backend)
   │                                                                      │
   ├─────── 1. HTTP GET /index.html ────────────────────────────────────►│
   │◄────── 2. 200 OK (Returns raw HTML file) ────────────────────────────┤
   │                                                                      │
   │  [ Browser parses HTML, discovers <link rel="stylesheet" href="style.css"> ]
   │  [ Discover <script src="app.js"> and <img src="logo.png">                ]
   │                                                                      │
   ├─────── 3. HTTP GET /style.css ──────────────────────────────────────►│
   ├─────── 4. HTTP GET /app.js ─────────────────────────────────────────►│
   ├─────── 5. HTTP GET /logo.png ───────────────────────────────────────►│
   │                                                                      │
   │◄────── 6. 200 OK (style.css data) ───────────────────────────────────┤
   │◄────── 7. 200 OK (app.js script data) ───────────────────────────────┤
   │◄────── 8. 200 OK (logo.png binary image) ────────────────────────────┤
   │                                                                      │
   ▼                                                                      ▼
[ Browser Executes JS, Applies CSS, Paints Pixels onto Screen ]

```

### Browser Rendering Sequence Steps:

1. **HTML Parsing (DOM Construction):** Converts raw HTML bytes into the Document Object Model (DOM) tree.
2. **CSS Parsing (CSSOM Construction):** Processes CSS rules to construct style models.
3. **Render Tree Creation:** Combines DOM and CSSOM to calculate what elements are actually visible.
4. **Layout (Reflow):** Calculates exact geometric positions (pixels, width, height) for every element.
5. **Painting:** Fills in pixels on the screen (colors, borders, text, images).
6. **JavaScript Execution:** JS engines (like Chrome's V8) execute code, modify the DOM dynamically, or trigger additional background requests (AJAX / Fetch API).

---

## Part 5 — Linux View 🐧

From a Linux perspective, a web server is simply a parent process listening on a socket, reading incoming HTTP stream data from file descriptors, and writing data back.

### Where Linux Stores Web Files & Logs:

* **Web Root (Static Files):** `/var/www/html/` (Default directory for Apache/Nginx static files).
* **Nginx Configuration:** `/etc/nginx/nginx.conf` or `/etc/nginx/sites-available/`.
* **Access Logs:** `/var/log/nginx/access.log` (Records every incoming HTTP request IP, URL, status code, and User-Agent).
* **Error Logs:** `/var/log/nginx/error.log` (Records backend execution crashes, missing file errors, or permission denials).

---

## Part 6 — Practical Demo 🛠️

Let's observe the full web page request cycle step-by-step from the command line using `curl`.

### Step 1: Trace DNS Resolution

```bash
dig +short example.com

```

* **Internal Action:** Sends a UDP query to Port 53 to translate `example.com` into its IP address (`93.184.215.14`).

### Step 2: Perform Verbose HTTP Request and Inspect the Full Flow

```bash
curl -v http://example.com

```

### Step 3: What Happens Internally After Hitting Enter:

1. `curl` resolves `example.com` to an IP.
2. Opens a TCP socket to `93.184.215.14:80`.
3. Sends raw HTTP request bytes over the TCP socket.
4. Reads the HTTP response header and body returned by the server.
5. Prints headers and HTML output directly to your terminal screen.

---

## Part 7 — Output Breakdown 🔍

When running `curl -v [http://example.com](http://example.com)`, here is the breakdown of what is happening under the hood:

```text
*   Trying 93.184.215.14:80...           <--- Step 1: Initiates TCP Connection
* Connected to example.com (93.184.215.14) port 80 (#0)

> GET / HTTP/1.1                         <--- Step 2: HTTP Request Line sent by client
> Host: example.com                       <--- Request Header: Target domain name
> User-Agent: curl/7.81.0                <--- Request Header: Client identification
> Accept: */*                            <--- Request Header: Allowed file types

< HTTP/1.1 200 OK                        <--- Step 3: Server Response Code
< Content-Type: text/html; charset=UTF-8 <--- Response Header: MIME type of data
< Content-Length: 1256                   <--- Response Header: Size of payload in bytes
< Date: Thu, 14 Aug 2026 00:00:00 GMT

<!doctype html>                          <--- Step 4: Body Data (Raw HTML content)
<html>
<head><title>Example Domain</title></head>
...

```

---

## Part 8 — Security Perspective (Red Team vs. SOC) ⚔️

### Red Team Operator Perspective 🎯

1. **Reconnaissance via Sub-requests:** When inspecting a web app, Red Teamers look at linked static assets (CSS, JS) in the HTML source code. Developers often accidentally leave sensitive comments, internal API endpoints, or staging server URLs inside public `.js` files!
2. **Server Fingerprinting:** Response headers like `Server: Apache/2.4.41` or `X-Powered-By: PHP/7.2.0` reveal backend software versions, helping operators look up known public vulnerabilities (CVEs).
3. **Dynamic Resource Exploitation:** Static files (`.png`, `.css`) rarely contain security flaws. Attacks (SQLi, RCE, Command Injection, IDOR) almost always target **Dynamic Resources** where user input is processed by backend code (e.g., `POST /login.php` or `GET /api/user?id=10`).

### SOC Analyst Perspective 🛡️

1. **Access Log Analysis:** SOC analysts analyze `/var/log/nginx/access.log` using tools like `grep`, `awk`, or SIEMs to spot web attacks. (e.g., searching for HTTP 500 errors or suspicious path patterns like `GET /etc/passwd`).
2. **Monitoring User-Agent Anomalies:** Attackers using automated tools (like `sqlmap`, `nikto`, or custom Python scripts) often leave default or suspicious `User-Agent` strings in the HTTP headers.
3. **Exfiltration via HTTP POST:** Large, abnormal HTTP `POST` requests originating from internal servers toward unknown external public IPs often signal data exfiltration.

---

## Part 9 — Common Beginner Mistakes ❌

1. **Mistake:** Assuming the browser downloads a complete web page in a single HTTP request.
* *Why it's wrong:* The initial request *only* fetches the raw HTML. The browser must make dozens or hundreds of separate HTTP requests for every image, script, stylesheet, and font referenced inside that HTML.


2. **Mistake:** Believing CSS and JavaScript run on the server.
* *Why it's wrong:* CSS and JavaScript are static assets downloaded to the client's machine and **executed locally inside the user's browser**. (Node.js is an exception on the backend, but standard browser JS runs locally).


3. **Mistake:** Confusing Static Resources with Dynamic Resources.
* *Why it's wrong:* Static resources are fixed files served directly from disk without modification. Dynamic resources execute backend programming logic, talk to databases, and build unique content per request.



---

## Part 10 — Professional Usage 💼

* **Red Team Operator:** Analyzes web request cycles to locate hidden parameters, inspect client-side JavaScript for leaked secrets, and craft custom HTTP payloads that bypass Web Application Firewalls (WAFs).
* **SOC Analyst:** Investigates web server access logs to detect directory brute-forcing, web shell interaction, credential stuffing, and unauthorized resource access.
* **Penetration Tester:** Assesses client-side dynamic execution, evaluates security headers, and tests API endpoints for authentication and authorization flaws.
* **Full-Stack Developer / Cloud Engineer:** Optimizes web asset delivery via CDNs (Content Delivery Networks), configures reverse proxies (Nginx), and builds scalable backend REST APIs.

---

## Part 11 — One-Page Summary 📝

```
=================================================================================
HOW THE WEB WORKS - MASTER SUMMARY
=================================================================================

1. CORE CYCLE:
   Client (Browser) ──[1. DNS Lookup]──► IP Address
   Client ────────────[2. TCP/TLS Handshake]──► Server (Port 80/443)
   Client ────────────[3. HTTP GET Request]───► Server
   Server ────────────[4. HTTP 200 Response]──► Client (Raw HTML)
   Client ────────────[5. Secondary Requests]─► Downloads CSS, JS, Images
   Client ────────────[6. Parsing & Render]───► Paints DOM/CSSOM to screen

2. RESOURCE TYPES:
   - Static: Fixed disk files (.html, .css, .js, .png) served without code execution.
   - Dynamic: Backend code (PHP, Python, Node) generated on-the-fly via DB queries.

3. RENDER STAGES:
   HTML Bytes ➔ DOM Tree ➔ CSSOM Tree ➔ Render Tree ➔ Layout ➔ Paint ➔ JS Execution

4. ESSENTIAL COMMANDS:
   - dig +short <domain>   : Resolve domain IP address.
   - curl -v http://target : Perform verbose HTTP request and print headers.
   - tail -f /var/log/nginx/access.log : Monitor web server requests in real time.
=================================================================================

```

---

## Part 12 — Reasoning & Thinking Quiz 🧠

Answer these scenarios based on technical reasoning:

1. **When you open a news website, your browser sends 1 primary HTTP request for `index.html`, but developer tools show a total of 85 network requests completed. Where did the other 84 requests come from?**
2. **If an attacker edits a site's `style.css` file locally inside their own browser's Developer Tools (F12) to make hidden admin buttons visible, does this change happen on the web server? Can they access the admin functionality? Explain why or why not.**
3. **Why do Red Team operators look for vulnerabilities (like SQL Injection or Command Injection) in dynamic backend endpoints (e.g., `/search.php?q=user`) rather than static files (e.g., `/logo.png`)?**
4. **If a web server's access log shows a client receiving an `HTTP 200 OK` response for `GET /app.js` followed immediately by `HTTP 500 Internal Server Error` for `POST /api/v1/login`, what does this tell you about where the failure occurred?**
5. **How does an Nginx web server know whether to serve a static file directly from `/var/www/html/photo.jpg` or forward the request to a Python/PHP application backend?**
