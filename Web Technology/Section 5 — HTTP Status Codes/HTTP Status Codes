# Section 4 — HTTP Methods (Verbs)

In HTTP, every single web action requires a **Method** (also called an **HTTP Verb**). The method is placed as the very first word in the Request Line to instruct the web server what type of action to perform on the target resource.

```
+─────────────────────────────────────────────────────────────────────────────+
|                             HTTP METHODS OVERVIEW                           |
+─────────────────────────────────────────────────────────────────────────────+
  GET     ──► Retrieve/Read data from server (Idempotent)[cite: 2]
  POST    ──► Submit/Create new data on server[cite: 2]
  PUT     ──► Completely overwrite/replace an existing resource[cite: 2]
  PATCH   ──► Partially update an existing resource[cite: 2]
  DELETE  ──► Remove/Destroy a resource on server[cite: 2]
  HEAD    ──► Fetch headers only (No body returned)[cite: 2]
  OPTIONS ──► Ask server which methods are allowed (Reconnaissance)[cite: 2]
+─────────────────────────────────────────────────────────────────────────────+

```

---

### 1. `GET` Method

* **What is it?**
The primary method used to request and retrieve data from a web server without modifying its state.


* **Why does it exist?**
To fetch web pages, images, stylesheets, API documents, and search results safely and repeatedly.


* **Real-life example:**
Reading a book from a public library bookshelf 📖. You look at the information, but you don't write in the book or destroy it.
* **Raw HTTP example:**
```http
GET /profile?user_id=101 HTTP/1.1
Host: target.com
User-Agent: Mozilla/5.0


```


* **What happens on the server?**
The web server parses the path and query parameters, reads the file or queries the database, and returns the data inside a `200 OK` response body.


* **How to recognize it:**
Triggered automatically whenever you click a link, type a URL into a browser address bar, or pass parameters inside `?key=value`.


* **Red Team perspective:**
Parameters sit directly inside the URL query string (`/user?id=1`). Attackers modify numeric IDs to test for **IDOR** (`/user?id=2`), inject SQL payloads into parameters, or test for **Reflected XSS**.


* **Small practical exercise:**
Run `curl -v "[http://httpbin.org/get?name=harish](http://httpbin.org/get?name=harish)"` and verify that the request line uses `GET` and sends no body payload.

---

### 2. `POST` Method

* **What is it?**
The method used to submit data to the server to create a new resource or process an action.


* **Why does it exist?**
To send sensitive, complex, or large data payloads (passwords, credit cards, file uploads) that should not appear in the URL query string.


* **Real-life example:**
Dropping a filled-out job application into a company drop-box 📥. A brand new applicant file is created on the server.
* **Raw HTTP example:**
```http
POST /api/v1/users HTTP/1.1
Host: target.com
Content-Type: application/json
Content-Length: 43

{"username": "harish0xRed", "role": "user"}

```


* **What happens on the server?**
The backend parses the body, validates input, writes a new record into the database, and typically responds with `201 Created` or `200 OK`.


* **How to recognize it:**
Triggered when submitting web forms (login screens, registration portals, payment pages) or making API creation requests.


* **Red Team perspective:**
Used to target authentication endpoints for **Credential Stuffing / Brute Forcing**, test for **Unrestricted File Upload** (uploading web shells), and probe for backend **SQL Injection / Deserialization** vulnerabilities.


* **Small practical exercise:**
Run `curl -v -X POST "[http://httpbin.org/post](http://httpbin.org/post)" -d "user=admin&pass=secret"` and inspect how the body data is received.

---

### 3. `PUT` vs. `PATCH` Methods

* **What is it?**
* `PUT`: Completely replaces/overwrites an existing resource with the provided payload.


* `PATCH`: Applies partial modifications/updates to an existing resource.




* **Why does it exist?**
To allow REST APIs to update existing records cleanly without needing to delete and recreate them.


* **Real-life example:**
* `PUT`: Tearing down a house and building a new house with the exact same house number 🏗️.
* `PATCH`: Repainting only the front door of the existing house 🚪.


* **Raw HTTP example:**
```http
PATCH /api/v1/user/101 HTTP/1.1
Host: target.com
Content-Type: application/json
Content-Length: 26

{"email": "new@red.com"}

```


* **What happens on the server?**
The backend finds object `101`, updates only the `email` column in the database, and leaves the remaining fields untouched.
* **How to recognize it:**
Observed in API networks (profile settings, updating item quantities in an e-commerce cart).


* **Red Team perspective:**
**Mass Assignment / Parameter Tampering:** An attacker sends `{"role": "admin"}` or `{"is_verified": true}` inside a `PATCH` request to escalate privileges if the backend blindly binds user input to internal database models.


* **Small practical exercise:**
Run `curl -v -X PATCH "[http://httpbin.org/patch](http://httpbin.org/patch)" -H "Content-Type: application/json" -d '{"status":"active"}'`.

---

### 4. `DELETE` Method

* **What is it?**
The method that instructs the server to remove or destroy the specified resource.


* **Why does it exist?**
To provide a standard HTTP mechanism for deleting records, users, files, or active sessions.


* **Real-life example:**
Tossing an old invoice into an office paper shredder 📄🔥.
* **Raw HTTP example:**
```http
DELETE /api/users/5 HTTP/1.1
Host: target.com
Cookie: session=harish_session[cite: 2]


```


* **What happens on the server?**
The server verifies authorization, executes a `DELETE FROM users WHERE id=5` database query, and returns `200 OK` or `204 No Content`.
* **How to recognize it:**
Triggered when clicking "Delete Account", "Remove Item", or logging out of modern web apps.
* **Red Team perspective:**
**Broken Object-Level Authorization (BOLA):** If the backend does not verify whether the requesting session owns the target object, an unprivileged user can send `DELETE /api/user/1` to wipe the administrator account!


* **Small practical exercise:**
Run `curl -v -X DELETE "[http://httpbin.org/delete](http://httpbin.org/delete)"`.

---

### 5. `HEAD` Method

* **What is it?**
Identical to a `GET` request, but the server returns **only the status line and headers**, with zero response body.


* **Why does it exist?**
To check if a resource exists, verify file size (`Content-Length`), or check cache timestamps without wasting bandwidth downloading large payloads.


* **Real-life example:**
Calling a warehouse to ask: *"Do you have the 500 MB file in stock?"* without asking them to deliver it over the truck 🚚.
* **Raw HTTP example:**
```http
HEAD /large-backup.tar.gz HTTP/1.1
Host: target.com


```


* **What happens on the server?**
The server processes the request, computes headers, sends back the header metadata, and immediately terminates the transmission without writing a body.


* **How to recognize it:**
Used by search engine crawlers, caching servers, and CDNs to check for updated content.
* **Red Team perspective:**
**Stealthy Server Fingerprinting:** Extracting the `Server`, `X-Powered-By`, and `Set-Cookie` headers from a target without generating large bandwidth footprints or downloading massive files.


* **Small practical exercise:**
Run `curl -I [http://example.com](http://example.com)` (`-I` uses `HEAD`) and notice how only headers are printed.

---

### 6. `OPTIONS` Method

* **What is it?**
Asks the server to return the list of allowed HTTP methods and communication options supported for a given URL.


* **Why does it exist?**
To allow clients (and browser CORS pre-flight checks) to determine what actions are permissible before sending an actual request.


* **Real-life example:**
Asking a restaurant receptionist: *"Do you accept cash, card, or UPI?"* before ordering food 📋.
* **Raw HTTP example:**
```http
OPTIONS /api/v1/posts HTTP/1.1
Host: target.com


```


* **What happens on the server?**
The server checks its routing table and responds with an `Allow` header listing supported verbs:
`Allow: GET, POST, PUT, DELETE, OPTIONS`

* **How to recognize it:**
Browsers send automatic `OPTIONS` pre-flight requests when making cross-origin (`fetch`/`AJAX`) API calls.
* **Red Team perspective:**
**Reconnaissance & Attack Surface Mapping:** Sending an `OPTIONS` request reveals if dangerous methods (like `PUT` or `DELETE`) are enabled on sensitive endpoints.


* **Small practical exercise:**
Run `curl -v -X OPTIONS [http://httpbin.org/get](http://httpbin.org/get)` and look for the `Allow:` header in the response.

---

### Red Team Method Tampering Note ⚔️

```
GET    /user?id=1      ──► Change to id=2 (Test for IDOR)[cite: 2]
POST   /login          ──► Body contains credentials (Target for Brute Force)[cite: 2]
DELETE /api/user/5     ──► Missing auth check? Delete arbitrary users[cite: 2]
OPTIONS /              ──► Returns "Allow: GET, POST, PUT, DELETE" (Expanded surface)[cite: 2]

```

---

Which specific HTTP method would you like to build custom requests for in our terminal lab, or are you ready to jump into **Section 5 — HTTP Status Codes**?
