# Module 3.2: URL Structure & Mechanics 🌐

Understanding how URLs are constructed and parsed by browsers, proxies, and web servers is essential for web reconnaissance, parameter manipulation, and exploitation.

---

## Part 1 — Big Picture 🌐

### The Problem

The Internet connects billions of machines holding trillions of individual files, API endpoints, and database records. To fetch a specific resource, a client needs a single, unambiguous address string that answers four questions:

1. **How** do I connect to it? (Protocol / Scheme)


2. **Where** is the server located on the network? (Host / Domain & Port)


3. **Which** specific resource or script on that server do I want? (Path)


4. **What** dynamic parameters or filters should the server apply? (Query String)



### What Happens Without Standardized URLs?

Without a standardized **Uniform Resource Locator (URL)**, every web application would invent its own proprietary addressing format, breaking hyperlinks, search engines, and cross-site resource sharing.

---

## Part 2 — Mental Model 🗺️📍

Think of a complete URL like an international postal address with apartment navigation instructions:

```
https://target.com:443/search?query=harish&page=2#results[cite: 2]
  │         │      │     │         │                │
  │         │      │     │         │                └── 6. FRAGMENT: Jump to specific bookmark on page[cite: 2]
  │         │      │     │         └─────────────────── 5. QUERY: Dynamic instructions/filters[cite: 2]
  │         │      │     └───────────────────────────── 4. PATH: Room/File inside building[cite: 2]
  │         │      └─────────────────────────────────── 3. PORT: Specific entry gate[cite: 1, 2]
  │         └────────────────────────────────────────── 2. HOST: Building name/address[cite: 2]
  └──────────────────────────────────────────────────── 1. SCHEME: Method of travel (Air mail / Armored car)[cite: 2]

```

* **Scheme (`https`):** The transport method (Encrypted Armored Delivery).


* **Host (`target.com`):** The target building in the city.


* **Port (`443`):** The specific door/gate to enter the building.


* **Path (`/search`):** The office room inside the building.


* **Query (`?query=harish&page=2`):** The custom work order handed to the person in that room.


* **Fragment (`#results`):** A sticky note you place on the document after reading it—it never leaves your hands.



---

## Part 3 — Internal Working & Component Breakdown ⚙️

```
+───────────────────────────────────────────────────────────────────────────────────────────+
|                                    URL COMPONENT MATRIX                                   |
+───────────────────────────────────────────────────────────────────────────────────────────+
| Component   | Example                  | Transmitted to Web Server? | Purpose             |
+=============+==========================+============================+=====================+
| Scheme      | https://                 | No (determines socket mode)| Transport protocol  |[cite: 2]
| Host        | target.com               | Yes (in Host header)       | Target destination  |[cite: 2]
| Port        | :443                     | No (used in TCP layer)     | Target TCP endpoint |[cite: 1, 2]
| Path        | /search                  | Yes (in Request Line)      | Endpoint resource   |[cite: 2]
| Query       | ?query=harish&page=2     | Yes (in Request Line)      | Dynamic parameters  |[cite: 2]
| Fragment    | #results                 | NO (Client browser only)   | Page scroll anchor  |[cite: 2]
+───────────────────────────────────────────────────────────────────────────────────────────+

```

### 1. Scheme / Protocol (`http://`, `https://`)

Tells the client which protocol and default port to use to open the transport connection.

* `http://` ➔ Plaintext TCP on Port 80.


* `https://` ➔ TLS-wrapped TCP on Port 443.


* `ftp://`, `ssh://`, `file://` ➔ Alternate protocols with distinct network behaviors.



### 2. Host (`target.com` or `192.168.1.50`)

The domain name or IP address of the target server. If a domain name is used, the client initiates a DNS lookup to resolve the IP address before establishing a TCP socket.

### 3. Port (`:80`, `:443`, `:8080`)

Specifies the destination port on the target server. If omitted from the URL, the client automatically defaults to the scheme's standard port (e.g., 80 for HTTP, 443 for HTTPS).

### 4. Path (`/search`, `/api/v1/users`)

Identifies the hierarchical file system path, route, or virtual endpoint on the server.

### 5. Query String (`?key=value&key2=value2`)

Begins with a `?` delimiter and contains key-value pairs separated by `&`. Used to send parameters to dynamic scripts via `GET` requests.

### 6. Fragment / Hash (`#section1`, `#results`)

Begins with a `#` delimiter. It is used strictly by the client-side browser to scroll down to a specific HTML anchor tag (`<div id="results">`) or used by JavaScript Single Page Applications (SPAs).

> ⚠️ **Key Security Rule:** The browser **never** sends the fragment `#` across the network to the web server!
> 
> 

---

## Part 4 — Real Packet Flow: How Browsers Parse URLs into HTTP 📡

When a user pastes `[https://target.com:443/search?query=harish&page=2#results](https://target.com:443/search?query=harish&page=2#results)` into a browser address bar, here is what actually happens on the wire:

```
User types full URL in Browser[cite: 2]
                │
                ▼
1. Browser extracts Scheme (https) ➔ Initializes TLS Handshake on Port 443[cite: 1, 2]
2. Browser extracts Host (target.com) ➔ Sets Host Header[cite: 2]
3. Browser extracts Path & Query ➔ Builds Request Line[cite: 2]
4. Browser strips Fragment (#results) ➔ Stays locally in browser memory[cite: 2]
                │
                ▼
[ TRANSMITTED ACROSS TCP/TLS SOCKET TO SERVER ]
GET /search?query=harish&page=2 HTTP/1.1[cite: 2]
Host: target.com[cite: 2]
User-Agent: Mozilla/5.0
Accept: */*

```

---

## Part 5 — Linux View & URL Encoding 🐧

URLs can only be transmitted over the internet using the **US-ASCII character set**. Any unsafe character (spaces, quotes, non-ASCII symbols) must be converted into **URL Encoding (Percent-Encoding)**:

* A space ` ` becomes `%20` or `+`
* A forward slash `/` becomes `%2F`
* An ampersand `&` becomes `%26`
* An equals sign `=` becomes `%3D`
* A hash `#` becomes `%23`

### Inspecting URLs with Linux Tools:

You can quickly URL-encode and decode strings directly in the Linux terminal:

```bash
# URL decode a string using python3
python3 -c "import urllib.parse; print(urllib.parse.unquote('admin%20user%26role%3Dadmin'))"

# URL encode a string
python3 -c "import urllib.parse; print(urllib.parse.quote('admin user&role=admin'))"

```

---

## Part 6 — Practical Demo 🛠️

Let's see how `curl` strips fragments and parses paths and query parameters.

### Step 1: Request an endpoint with a Query String

```bash
curl -v "http://httpbin.org/get?user=harish&role=operator"

```

### Step 2: Observe the Wire Request Line

Inside the verbose output (`>`), you will see the exact path and query string sent together in the Request Line:

```http
GET /get?user=harish&role=operator HTTP/1.1
Host: httpbin.org
User-Agent: curl/7.81.0
Accept: */*

```

### Step 3: Test a URL containing a Fragment

```bash
curl -v "http://httpbin.org/get?user=harish#secretFragment"

```

Look at the request line sent by `curl`:

```http
GET /get?user=harish HTTP/1.1

```

Notice that `#secretFragment` was discarded entirely before transmission!

---

## Part 7 — Output Breakdown 🔍

When interacting with query strings, the server's backend extracts variables from the path:

```json
{
  "args": {
    "role": "operator", 
    "user": "harish"
  }, 
  "headers": {
    "Host": "httpbin.org", 
    "User-Agent": "curl/7.81.0"
  }, 
  "url": "http://httpbin.org/get?user=harish&role=operator"
}

```

* **`args`:** The dictionary of parsed key-value query parameters extracted by the server backend.
* **`url`:** The full reconstructed URL on the server side.

---

## Part 8 — Security Perspective (Red Team vs. SOC) ⚔️

### Red Team Operator Perspective 🎯

1. **Insecure Direct Object Reference (IDOR):**
* Manipulating numeric query parameters (e.g., changing `/api/user?id=100` to `/api/user?id=101`) to access unauthorized data when authorization checks are missing.




2. **Path Traversal / Local File Inclusion (LFI):**
* Injecting dot-dot-slash patterns into the path (e.g., `GET /view?file=../../../../etc/passwd`) to read system files from disk.


3. **Open Redirects:**
* Targeting unvalidated redirect parameters (e.g., `GET /login?redirect=[https://evil-attacker.com](https://evil-attacker.com)`) to trick users into navigating to phishing sites.


4. **DOM-based XSS via Fragment:**
* Because the fragment `#` stays in the client browser, malicious JavaScript payloads placed in the URL hash (e.g., `[https://target.com/page#](https://target.com/page#)<script>alert(1)</script>`) are executed by client-side JS without ever touching the web server or triggering backend WAF rules!



### SOC Analyst Perspective 🛡️

1. **URL Parameter Fuzzing Alerts:** Detecting rapid bursts of requests targeting paths like `/user?id=1`, `/user?id=2`, `/user?id=3` (indicating automated IDOR scanning or scraping).
2. **Encoded Attack Detection:** Monitoring for double-URL-encoded characters (like `%252e%252e%252f` representing `../`) used to bypass web application firewall path filters.
3. **Referer Header Tracking:** Tracking the `Referer` header to identify external domains referring malicious traffic into internal endpoints.



---

## Part 9 — Common Beginner Mistakes ❌

1. **Expecting the Server to See URL Fragments:**
* *Mistake:* Believing a server can process or log the `#fragment` portion of a URL.


* *Why it's wrong:* The browser explicitly strips everything from `#` onward before creating the HTTP request line.




2. **Confusing Path Parameters with Query Strings:**
* *Path parameter:* `/users/15/profile`
* *Query parameter:* `/users/profile?id=15`

* Both pass data to the backend, but require different routing and testing methodologies.


3. **Forgetting to URL-Encode Special Characters in Exploits:**
* If you inject an unencoded `&` or `#` inside a parameter payload, the web server treats `&` as a new parameter and `#` as a comment/fragment, truncating and breaking your exploit payload!





---

## Part 10 — Professional Usage 💼

* **Red Team Operator:** Uses parameter fuzzing tools (`ffuf`, `wfuzz`) to discover hidden URL paths and endpoints, injects payloads into query strings, and discovers IDOR vulnerabilities.


* **SOC Analyst:** Analyzes web proxy logs (Squid, Zscaler) to inspect full URLs requested by compromised endpoints to detect C2 callbacks and phishing links.
* **Penetration Tester:** Tests URL parser discrepancies between frontend reverse proxies and backend servers to bypass access control lists (ACLs).

---

## Part 11 — One-Page Summary 📝

```
=================================================================================
SUBHEADING 3.2: URL STRUCTURE & MECHANICS MASTER SHEET
=================================================================================

1. URL ANATOMY:
   scheme://host:port/path?query_string#fragment[cite: 2]
   
   - Scheme   : Protocol (http, https, ftp)[cite: 2]
   - Host     : Target Domain or IP Address[cite: 2]
   - Port     : TCP Port (Default: 80 for HTTP, 443 for HTTPS)[cite: 1, 2]
   - Path     : Target resource / endpoint on the server[cite: 2]
   - Query (?) : Key-value data parameters (& separated)[cite: 2]
   - Fragment (#): Client-side anchor (NEVER sent over network wire!)[cite: 2]

2. WIRE TRANSFORMATION:
   URL: https://target.com/search?q=test#top[cite: 2]
   Wire Request Line: GET /search?q=test HTTP/1.1[cite: 2]
   Host Header: target.com[cite: 2]

3. ATTACK PATTERNS:
   - Path     -> Directory Traversal (/../../etc/passwd), LFI
   - Query    -> IDOR (?id=2), SQLi, XSS, SSRF, Open Redirects[cite: 2]
   - Fragment -> DOM-Based XSS (Handled purely by browser JS)
=================================================================================

```

---

## Part 12 — Reasoning & Thinking Quiz 🧠

Answer these scenarios to verify your understanding of URL mechanics:

1. If a user navigates to `[https://bank.com/account#transfer-money](https://bank.com/account#transfer-money)`, does the string `#transfer-money` ever appear in the web server's `/var/log/nginx/access.log`? Explain why or why not.


2. **If an attacker crafts a SQL injection payload containing a space and a comment character (e.g., `' OR 1=1 -- `), why must those characters be URL-encoded before sending them inside a URL query string?**
3. An application serves user profiles via `[https://app.com/profile?id=50](https://app.com/profile?id=50)`. If an attacker changes `id=50` to `id=51` and successfully views another user's private dashboard without logging into their account, what specific vulnerability does this represent?


4. Why does typing `[http://example.com:8080/](http://example.com:8080/)` cause the browser to send a request to TCP port 8080 instead of the standard default port 80?


5. **If an application accepts a parameter `redirect_url` (e.g., `[https://target.com/logout?redirect_url=https://attacker.com](https://target.com/logout?redirect_url=https://attacker.com)`), what type of web vulnerability should a penetration tester test for?**
