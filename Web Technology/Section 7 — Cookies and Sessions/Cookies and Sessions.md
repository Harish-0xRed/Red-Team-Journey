# Section 7 — Cookies and Sessions

Cookies and sessions form the state-management layer of the web, making them the most frequently targeted components in web application security.

---

**What a Cookie Is**

A cookie is a small piece of text stored by the browser and transmitted to the server with every subsequent request:

* **Server directive:** `Set-Cookie: session=a1b2c3d4; HttpOnly; Secure; SameSite=Lax`

* **Browser storage:** Stores `session=a1b2c3d4` locally for that domain.


* **Subsequent requests:** `Cookie: session=a1b2c3d4` is sent automatically in the request headers.



---

**Cookie Flags & Security Properties**

| Flag | Function | Risk if Missing / Misconfigured |
| --- | --- | --- |
| **`HttpOnly`**<br> | Prevents client-side JavaScript (`document.cookie`) from accessing the cookie.

 | **Cookie Theft via XSS:** Injected scripts can read the cookie and transmit it to an attacker.

 |
| **`Secure`**<br> | Restricts cookie transmission strictly to encrypted HTTPS connections.

 | **Cleartext Sniffing:** Cookies sent over unencrypted HTTP can be intercepted on the local network.

 |
| **`SameSite=Lax`**<br> | Limits cross-site cookie transmission to top-level navigations.

 | **Cross-Site Request Forgery (CSRF):** Malicious third-party sites can trigger state-changing actions.

 |
| **`SameSite=Strict`**<br> | Completely blocks cookie transmission in all cross-site contexts.

 | Provides strong CSRF protection across all third-party origins.

 |

---

**Session vs. Cookie Architecture**

* **Cookie:** Stored in the browser; contains the session identifier (acting as a key).


* **Session:** Stored on the server; contains the actual user profile, permissions, and authentication state.


* **Authentication Impact:** Stealing a valid session cookie gives the attacker the key, granting access to the authenticated session without requiring credentials.



```
Client (Browser)                                         Server
┌──────────────────────┐                         ┌───────────────────────┐
│ Stored Cookie:       │                         │ Session Store:        │
│ session=a1b2c3d4     │ ─── Cookie Header ────► │ a1b2c3d4 ──► User: 10 │[cite: 2]
└──────────────────────┘                         └───────────────────────┘

```

---

**JSON Web Tokens (JWT)**

Modern stateless architectures often use JWTs instead of server-side session databases.

* **Structure:** Three Base64URL-encoded segments separated by dots:

$$\text{header}.\text{payload}.\text{signature} \text{[cite: 2]}$$


* **Payload Inspection:**
```bash
echo "eyJ1c2VyIjoiaGFyaXNoIn0=" | base64 -d
# Output: {"user":"harish"}[cite: 2]

```


* **Attack Angle:** If the server fails to verify the cryptographic signature properly, modifying payload claims (e.g., changing `"user": "harish"` to `"user": "admin"`) results in privilege escalation.



---

**Practical Terminal Exercise**

Use `curl` to observe and decode a base64-encoded cookie payload:

```bash
# Decode a Base64-encoded session string
echo "eyJ1c2VyIjoiaGFyaXNoIiwicm9sZSI6ImFkbWluIn0=" | base64 -d

```

---

**Reasoning Quiz**

1. If an application sets a session cookie without the `HttpOnly` flag, how can an attacker leverage a Stored XSS vulnerability on the comment section to achieve full account takeover?


2. Why does changing claims inside a JWT payload fail if the server correctly validates the signature using a secret private key?


3. What is the difference between where a **session** lives and where a **cookie** lives?
