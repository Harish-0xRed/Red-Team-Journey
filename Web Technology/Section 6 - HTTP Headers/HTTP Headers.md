# Section 6 — HTTP Headers

HTTP headers carry the metadata of every request and response, serving as a primary starting point for red team reconnaissance and exploitation.

---

### Request Headers (Client to Server)

| Header | What It Does | Attack Angle |
| --- | --- | --- |
| **`Host`**<br> | Specifies the target domain on the server.

 | **Host Header Injection:** Cache poisoning, password reset poisoning, or routing to internal virtual hosts.

 |
| **`User-Agent`**<br> | Identifies the client browser, OS, and version.

 | **WAF / Filter Bypass:** Spoofing headers (e.g., impersonating Googlebot) to access restricted content.

 |
| **`Cookie`**<br> | Sends stored session tokens to the server.

 | **Session Hijacking:** Stealing or replaying session cookies to take over accounts.

 |
| **`Authorization`**<br> | Transmits credentials or bearer tokens (e.g., `Bearer <JWT>`).

 | **Token Abuse:** JWT manipulation, brute forcing, or privilege escalation via forged tokens.

 |
| **`Content-Type`**<br> | Declares the MIME format of the request body.

 | **Content-Type Confusion / File Upload Bypass:** Switching between `application/json` and `application/x-www-form-urlencoded` to bypass parsers.

 |
| **`Referer`**<br> | Indicates the previous page URL the user navigated from.

 | **Information Leakage:** Exposes internal endpoints, sensitive parameters, or staging URLs.

 |
| **`X-Forwarded-For`**<br> | Identifies the originating client IP when routed through proxies.

 | **IP Spoofing / ACL Bypass:** Setting `X-Forwarded-For: 127.0.0.1` to access restricted administrative panels.

 |
| **`Accept-Language`**<br> | Declares the client's preferred language.

 | **Behavior Tampering:** Alters backend response localization, error output formats, or template handling.

 |

---

### Response Headers (Server to Client)

| Header | What It Does | Attack Angle |
| --- | --- | --- |
| **`Set-Cookie`**<br> | Instructs the browser to store a cookie.

 | **Missing Flags:** If `HttpOnly` is missing, cookies can be stolen via XSS; if `Secure` is missing, cookies can leak over unencrypted HTTP.

 |
| **`Location`**<br> | Specifies the target URL for redirects.

 | **Open Redirect:** Manipulating parameter inputs to force redirection to external malicious sites.

 |
| **`Server`**<br> | Identifies web server software and version.

 | **Fingerprinting:** Identifies specific server builds to locate known public CVEs.

 |
| **`X-Powered-By`**<br> | Discloses backend framework and version.

 | **Stack Fingerprinting:** Identifies backend environments (e.g., PHP 7.2.0, Express, Laravel) to map the attack surface.

 |
| **`Content-Security-Policy`**<br> | Restricts sources for executable scripts and resources.

 | **XSS Exploitation:** If missing or improperly configured (`unsafe-inline`), Cross-Site Scripting payloads execute unrestricted.

 |
| **`Access-Control-Allow-Origin`**<br> | Defines Cross-Origin Resource Sharing (CORS) rules.

 | **CORS Misconfiguration:** Setting `*` with credentials allows external sites to steal authenticated data.

 |
| **`Strict-Transport-Security`**<br> | Enforces HTTPS communication.

 | **SSL Stripping:** If missing, connections can be downgraded to unencrypted HTTP.

 |

---

### Analyzing Response Headers

```http
HTTP/1.1 200 OK
Server: Apache/2.4.41                <-- Apache version -> search CVEs for this version[cite: 2]
X-Powered-By: PHP/7.2.0             <-- PHP 7.2 -> old version, many known CVEs[cite: 2]
Set-Cookie: session=abc123           <-- No HttpOnly flag -> vulnerable to XSS theft[cite: 2]
Content-Security-Policy: (missing)  <-- No CSP -> XSS possible[cite: 2]

```

These four header lines reveal the server software, backend runtime, exposure to cookie theft, and absence of XSS protections before launching an active exploit.

---

### Practical Lab: Header Inspection with `curl`

Inspect response headers directly in the terminal using `curl -I`:

```bash
# 1. Fetch only response headers
curl -I https://httpbin.org/get

# 2. Send custom request headers (Spoofing User-Agent and X-Forwarded-For)
curl -v https://httpbin.org/headers \
     -H "User-Agent: Googlebot/2.1" \
     -H "X-Forwarded-For: 127.0.0.1"

```

---

### Key Technical Questions

1. How does setting `X-Forwarded-For: 127.0.0.1` enable an attacker to bypass an IP-based restriction on an administrative panel?


2. If a response header contains `Set-Cookie: session=xyz987; Secure`, but lacks `HttpOnly`, what attack is still possible?


3. What reconnaissance value does the `X-Powered-By` header provide during the initial enumeration phase?
