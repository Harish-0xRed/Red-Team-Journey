# Section 5 — HTTP Status Codes

HTTP status codes are the server's standardized 3-digit response to a client's request. They tell you immediately whether the request succeeded, redirected, was rejected, or crashed the backend server.

```
+─────────────────────────────────────────────────────────────────────────────+
|                         HTTP STATUS CODE RANGES                             |
+─────────────────────────────────────────────────────────────────────────────+
  1xx  ──► Informational : "Request received, continuing process..."[cite: 2]
  2xx  ──► Success       : "Action understood and accepted"[cite: 2]
  3xx  ──► Redirection   : "Further action needed to complete request"[cite: 2]
  4xx  ──► Client Error  : "Malformed syntax or unauthorized access"[cite: 2]
  5xx  ──► Server Error  : "Server failed to fulfill a valid request"[cite: 2]
+─────────────────────────────────────────────────────────────────────────────+

```

---

### The 5 Core Status Code Ranges

| Range | Class | Plain-Language Meaning | Red Team / SOC Relevance |
| --- | --- | --- | --- |
| **`1xx`** | Informational | *"Hold on, still working..."*<br> | Protocol upgrades (`101 Switching Protocols` to WebSockets). |
| **`2xx`** | Success | *"Got it, here is your data."*<br> | Valid endpoint, clean execution, baseline response.

 |
| **`3xx`** | Redirection | *"The resource moved somewhere else."*<br> | Open Redirect attacks, finding hidden internal endpoints.

 |
| **`4xx`** | Client Error | *"You made a mistake or lack permission."*<br> | Directory fuzzing, authentication bypass testing, WAF drops.

 |
| **`5xx`** | Server Error | *"The backend code/server crashed."*<br> | **High-value injection indicator** (SQLi, SSTI, RCE).

 |

---

### Most Important Status Codes for Red Teaming

**`200 OK`**

* **Meaning:** The request succeeded and the payload is returned in the response body.


* **Red Team Relevance:** The baseline normal state. Content-length variations on 200 responses help identify boolean-based SQL injection and directory bruteforcing hits.



**`201 Created`**

* **Meaning:** The request succeeded and a new resource was created on the server.


* **Red Team Relevance:** Seen on successful user registration, file uploads, or API object creation.



**`301 Moved Permanently` vs. `302 Found (Temporary Redirect)**`

* **Meaning:** The requested resource has moved. The server includes a `Location:` header telling the client where to go next.


* **Red Team Relevance:** `302` redirects are prime targets for **Open Redirect vulnerabilities** if the destination parameter is controllable (e.g., `/login?next=[https://evil.com](https://evil.com)`).



**`400 Bad Request`**

* **Meaning:** The server cannot parse the request due to invalid syntax or oversized headers.


* **Red Team Relevance:** Useful when fuzzing API schema parameters or testing HTTP Request Smuggling payloads.



**`401 Unauthorized`**

* **Meaning:** Authentication is required and has failed or has not yet been provided (missing token or Basic Auth header).


* **Red Team Relevance:** Marks the presence of a login gate. Targets for credential stuffing and brute-forcing.



**`403 Forbidden`**

* **Meaning:** The server understands the request, but **refuses to authorize it**.


* **Red Team Relevance:** **The resource EXISTS, but you are blocked**. This is an invitation to test **403 Bypasses** (manipulating headers like `X-Forwarded-For: 127.0.0.1`, changing case `/admin` to `/Admin`, or using URL path normalization tricks `/admin/..;/admin`).



**`404 Not Found`**

* **Meaning:** The server cannot find the requested resource.


* **Red Team Relevance:** The endpoint does not exist. Stop targeting that exact path and move on in directory discovery.



**`405 Method Not Allowed`**

* **Meaning:** The resource exists, but the HTTP method used (`GET`, `POST`, `DELETE`) is disabled.


* **Red Team Relevance:** Change verbs! If `GET /api/user/1` returns `405`, try `POST`, `PUT`, or `PATCH` to access the endpoint.



**`500 Internal Server Error`**

* **Meaning:** An unhandled exception or crash occurred in the backend application code.


* **Red Team Relevance:** **Goldmine**. Indicates your input broke backend execution logic (syntax errors in database queries, unhandled null pointers, or command injection crashes).



**`502 Bad Gateway` / `504 Gateway Timeout**`

* **Meaning:** The frontend reverse proxy (like Nginx) cannot reach or timed out waiting for the backend application server (like Node.js or Python).


* **Red Team Relevance:** Can indicate successful Denial of Service (DoS) or a backend hanging due to a time-based blind SQL injection payload (e.g., `pg_sleep(10)`).

---

### Critical Red Team Differences

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       CRITICAL TRIAGE DIFFERENCES                           │
├─────────────────────────────────────────────────────────────────────────────┤
│ 403 Forbidden  ──► Resource EXISTS, but permissions blocked[cite: 2].              │
│                    Action: Try bypass techniques (Headers, IP spoofing,     │
│                    path variations)[cite: 2].                                      │
├─────────────────────────────────────────────────────────────────────────────┤
│ 404 Not Found  ──► Resource DOES NOT EXIST on server[cite: 2].                     │
│                    Action: Fuzz a different wordlist path[cite: 2].                │
├─────────────────────────────────────────────────────────────────────────────┤
│ 500 Error      ──► Your input broke something inside the backend[cite: 2].         │
│                    Action: Dig deeper into the parameter — injection point  │
│                    likely present[cite: 2]!                                        │
└─────────────────────────────────────────────────────────────────────────────┘

```

---

### Practical Demonstration with `curl`

Test how web servers emit different status codes using `httpbin.org`:

```bash
# 1. Test a 200 Success
curl -I https://httpbin.org/status/200

# 2. Test a 302 Redirect (Notice the Location header)
curl -I https://httpbin.org/status/302

# 3. Test a 403 Forbidden (Examine the blocked response)
curl -I https://httpbin.org/status/403

# 4. Test a 500 Backend Crash
curl -I https://httpbin.org/status/500

```

---

### Reasoning Quiz 🧠

1. Why is receiving an `HTTP 403 Forbidden` often more interesting to a penetration tester during directory discovery than receiving an `HTTP 404 Not Found`?


2. If sending a single quote (`'`) inside a search query parameter changes the server's response from `200 OK` to `500 Internal Server Error`, what type of vulnerability might you have found?


3. If `GET /api/v1/deleteUser` returns `405 Method Not Allowed`, what should your immediate next test be?
