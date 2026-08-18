# Module: HTTP vs HTTPS (Plaintext vs TLS Encryption) 🔒📄

---

## Part 1 — Big Picture 🌐

### What is HTTP vs HTTPS?

* **HTTP (Port 80):** Transmits application data across the network as raw, unencrypted ASCII text.


* **HTTPS (Port 443):** Takes the exact same HTTP request/response text and wraps it inside a cryptographic tunnel negotiated by **TLS (Transport Layer Security)**.



### The Problem it Solves

The Internet is a public mesh of untrusted routers, ISP switches, and public Wi-Fi access points. When using plain HTTP, any intermediary device along the path can view, intercept, log, or tamper with the data. HTTPS provides three critical guarantees:

1. **Confidentiality:** Eavesdroppers cannot read credentials, cookies, or payload data.


2. **Integrity:** Data cannot be modified or injected with malicious scripts in transit without detection.
3. **Authentication:** Proves the client is genuinely connected to the intended domain server via digital certificates, preventing server impersonation.

---

## Part 2 — Mental Model ✉️📦

Imagine sending sensitive personal financial documents through the postal system:

```
[ HTTP (The Postcard) ✉️ ]
  Sender writes: "Username: admin, Password: Password123"[cite: 2]
         │
         ▼ (Carried openly by postal workers, drivers, sorting offices)
  Anyone who picks up the postcard can read the entire message directly[cite: 2].

-------------------------------------------------------------------------

[ HTTPS (The Armored, Tamper-Evident Lockbox) 🔐 ]
  Sender puts the same message inside a hardened, digitally locked titanium box[cite: 2].
         │
         ▼ (Carried across the same postal route)
  Postal workers only see:
    - Origin Address (Source IP / Client)[cite: 1]
    - Destination Address (Destination IP: Port 443)[cite: 1, 2]
    - Scrambled, undecipherable ciphertext inside[cite: 2]

```

---

## Part 3 — Internal System Flow ⚙️

### Network Layer Stack Comparison

```
+--------------------------+          +--------------------------+
|      HTTP Protocol       |          |      HTTPS Protocol      |
+--------------------------+          +--------------------------+
|  Application: HTTP       |          |  Application: HTTP       |
+--------------------------+          +--------------------------+
|            ──            |          |  Security: TLS (Crypto)  |
+--------------------------+          +--------------------------+
|  Transport: TCP (Port 80)|          |  Transport: TCP (Port 443|
+--------------------------+          +--------------------------+
|  Network: IP             |          |  Network: IP             |
+--------------------------+          +--------------------------+
|  Data Link: Ethernet     |          |  Data Link: Ethernet     |
+--------------------------+          +--------------------------+

```

### The HTTPS Connection Lifecycle

```
[ Client Browser ]                                            [ Target Server ]
       │                                                              │
       │ 1. TCP 3-Way Handshake (SYN ➔ SYN-ACK ➔ ACK)                 │
       ├─────────────────────────────────────────────────────────────►│
       │                                                              │
       │ 2. TLS Client Hello (Supported ciphers, SNI: target.com)     │
       ├─────────────────────────────────────────────────────────────►│
       │                                                              │
       │ 3. TLS Server Hello + Server Digital Certificate             │
       │◄─────────────────────────────────────────────────────────────┤
       │                                                              │
       │ 4. Client verifies Certificate & exchanges symmetric keys    │
       ├─────────────────────────────────────────────────────────────►│
       │                                                              │
       │ ════════════ ENCRYPTED TLS TUNNEL ESTABLISHED ══════════════ │
       │                                                              │
       │ 5. Encrypted HTTP GET /login HTTP/1.1                        │
       ├─────────────────────────────────────────────────────────────►│
       │                                                              │
       │ 6. Encrypted HTTP 200 OK + HTML Body                         │
       │◄─────────────────────────────────────────────────────────────┤

```

---

## Part 4 — Real Packet Flow: Wire Comparison 📡

What an attacker sees on the wire when capturing packets with `tcpdump` or Wireshark:

### Over Plain HTTP (Port 80):

```http
POST /login.php HTTP/1.1
Host: testphp.vulnweb.com
User-Agent: Mozilla/5.0
Content-Type: application/x-www-form-urlencoded
Cookie: session_id=e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855[cite: 2]

uname=harish&pass=SecretAdmin2026[cite: 2]

```

> **Vulnerability:** The cookie, request path, parameters, and plaintext password are fully readable.
> 
> 

### Over HTTPS (Port 443):

```text
Frame 42: 384 bytes on wire
Transmission Control Protocol, Src Port: 54210, Dst Port: 443
Transport Layer Security:
    TLSv1.3 Record Layer: Application Data Protocol: http-over-tls
    Encrypted Application Data: 1a7f8b90c42d3e5f11a8b9...[cite: 2]

```

> **Protection:** Only the source/destination IPs, ports, and TLS handshake metadata are visible. The entire HTTP request (method, headers, cookies, POST body) is encrypted ciphertext.
> 
> 

---

## Part 5 — Linux View 🐧

### 1. Trusted Certificate Authorities in Linux

When your Linux machine connects via HTTPS, it verifies the server's certificate against a local store of trusted root Certificate Authorities (CAs):

* **Debian/Ubuntu:** `/etc/ssl/certs/ca-certificates.crt`
* **RHEL/CentOS:** `/etc/pki/tls/certs/ca-bundle.crt`

If an internal enterprise proxy or attacker uses an untrusted certificate, tools like `curl` fail with:
`curl: (60) SSL certificate problem: self-signed certificate in certificate chain`

### 2. Linux Native TLS Tooling

* `nc` connects directly to raw TCP sockets, but **cannot handle TLS handshakes**.


* `openssl s_client` is the standard Linux utility for establishing raw TLS connections and inspecting certificate chains.



---

## Part 6 — Practical Demo 🛠️

Let's test the difference between plaintext connection and TLS connection directly from the Linux terminal using the Bandit foundation principles.

### Experiment 1: Raw Plaintext with `nc` (Port 80)

```bash
# Connect to a plaintext web server
nc example.com 80

# Send request:
GET / HTTP/1.1
Host: example.com


```

* **Result:** The connection immediately returns readable ASCII headers and HTML.



---

### Experiment 2: Attempting `nc` on an HTTPS Port (Port 443)

```bash
# Connect to an encrypted port with nc
nc example.com 443

# Send the same request:
GET / HTTP/1.1
Host: example.com


```

* **Result:** Nothing happens, or the connection is immediately terminated. The server on port 443 expects a cryptographic TLS `ClientHello` handshake, not raw ASCII text!



---

### Experiment 3: Raw Encrypted HTTP with `openssl s_client` (Port 443)

```bash
# Connect to the TLS port using OpenSSL
openssl s_client -connect example.com:443 -quiet

```

Once connected, type your HTTP request and hit Enter twice:

```http
GET / HTTP/1.1
Host: example.com


```

* **Result:** `openssl` manages the TLS handshake, encrypts your text on transmission, decrypts the server's response, and prints the plaintext HTTP response.



---

## Part 7 — Output Breakdown: The TLS Handshake Trace 🔍

Running `openssl s_client -connect example.com:443` reveals what happens before any HTTP bytes travel:

```text
CONNECTED(00000003)
---
Certificate chain
 0 s:CN = example.com                           <--- Server identity
   i:C = US, O = DigiCert Inc, CN = DigiCert... <--- CA that signed it
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIFazCCA1OgAwIBAgIQD/hP4...                   <--- Public Key Certificate
-----END CERTIFICATE-----
subject=CN = example.com
issuer=C = US, O = DigiCert Inc, CN = DigiCert...
---
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384  <--- Agreed encryption algorithm
Server public key is 256 bit
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 0 (ok)                      <--- Validation successful
---

```

---

## Part 8 — Security Perspective (Red Team vs SOC) ⚔️

### Red Team Operator Perspective 🎯

1. **Cleartext Credential Harvesting (HTTP):** In legacy or misconfigured enterprise environments with internal HTTP web portals, operators execute ARP poisoning (`arpspoof`) or LLMNR/NBT-NS poisoning to capture session tokens and credentials out of the air.


2. **SSL Stripping (`sslstrip`):** When victims navigate to an unencrypted HTTP URL, operators sitting in the middle intercept the `301/302 Redirect` to HTTPS, serving the victim plain HTTP while maintaining an HTTPS connection to the real server.
3. **C2 over HTTPS:** Red Teamers tunnel Command & Control (C2) agent beacons inside encrypted HTTPS (Port 443) connections. This blends malicious traffic into standard corporate web browsing, blinding basic network intrusion detection systems (NIDS).



### SOC Analyst Perspective 🛡️

1. **TLS Inspection / SSL Decryption Proxies:** To combat encrypted malware traffic, enterprise SOCs route internal traffic through Next-Gen Firewalls (NGFW) that terminate and inspect TLS traffic using an enterprise-installed root CA before re-encrypting it.
2. **JA3 / JA4 Fingerprinting:** Because client applications (e.g., Python `requests`, Metasploit, standard Chrome) construct their `ClientHello` packet with unique combinations of ciphers and extensions, defenders identify malicious tools even when the payload is encrypted.
3. **HSTS Enforcement Monitoring:** Defenders enforce **HSTS (HTTP Strict Transport Security)** headers (`Strict-Transport-Security: max-age=31536000; includeSubDomains; preload`) to ensure browsers reject any connection attempting to downgrade to plaintext HTTP.



---

## Part 9 — Common Beginner Mistakes ❌

1. **Mistake: "HTTPS means the website is completely safe."**

* *Why it's wrong:* HTTPS only guarantees that the connection between you and the server is encrypted and authenticated. Phishing websites, malware download servers, and scam portals use valid, free HTTPS certificates (such as Let's Encrypt) every day.




2. **Mistake: Assuming HTTPS hides the domain name you are visiting.**
* *Why it's wrong:* During the initial TLS handshake, the client sends the domain name in plaintext inside the **SNI (Server Name Indication)** field of the `ClientHello` packet so the server knows which SSL certificate to return. Network observers can still see the domain name, but not the specific page path or payload.




3. **Mistake: Thinking HTTPS replaces backend authentication.**
* *Why it's wrong:* Encryption protects data *in transit*. If your web application has an SQL injection, IDOR, or broken access control bug, HTTPS will encrypt the attacker's exploit payload just as efficiently as valid data!





---

## Part 10 — Professional Usage 💼

* **Red Team Operator:** Leverages HTTPS with legitimate domain fronting or custom redirectors to deliver payloads and maintain persistence without triggering network-level signature alerts.
* **SOC Analyst:** Identifies rogue endpoints failing certificate validation checks and analyzes encrypted traffic patterns via NetFlow, SNI logs, and JA4 signatures to uncover active intrusions.
* **Penetration Tester:** Intercepts HTTPS traffic locally by installing a trusted CA certificate into Burp Suite, allowing full inspection and tampering of encrypted web traffic on the client side.
* **Security Engineer:** Deploys automated TLS certificate rotation (via ACME / Let's Encrypt), configures modern cipher suites, and enforces HSTS to prevent downgrade vulnerabilities.



---

## Part 11 — One-Page Summary 📝

```
=================================================================================
HTTP VS HTTPS MASTER REFERENCE SHEET
=================================================================================

1. FUNDAMENTAL DIFFERENCES:
   - HTTP  : Plaintext ASCII on TCP Port 80. No encryption. Sniffable on wire[cite: 1, 2].
   - HTTPS : Encrypted HTTP inside a TLS wrapper on TCP Port 443[cite: 1, 2].

2. SECURITY TRIAD PROVIDED BY HTTPS:
   - Confidentiality: Data scrambled with symmetric session keys (e.g., AES-GCM)[cite: 2].
   - Integrity      : Tamper-evident message authentication codes (MAC/HMAC).
   - Authentication : Server identity validated via X.509 Digital Certificates.

3. COMMANDS & TESTING:
   - nc target.com 80                        : Test raw plaintext HTTP[cite: 2].
   - openssl s_client -connect target:443    : Test raw encrypted HTTPS connection[cite: 2].
   - curl -Iv https://target.com             : View response headers and TLS details.
=================================================================================

```

---

## Part 12 — Reasoning & Thinking Quiz 🧠

Answer these scenarios based on technical reasoning:

1. If you enter your banking password on an unencrypted HTTP page (`[http://bank.com/login](http://bank.com/login)`) while connected to airport Wi-Fi, which entities between your laptop and the bank's server can read your password?


2. When connecting to `[https://google.com](https://google.com)`, an eavesdropper cannot see the path `/search?q=secret`. Can they still see that your machine is connecting to `google.com`? Explain how (mention SNI and DNS).


3. In the Bandit wargame, why did Level 14 allow you to submit the password using `nc`, but Level 15 required `openssl s_client`?


4. An attacker attempts an SSL-stripping attack against a user visiting `[http://example.com](http://example.com)`. What HTTP response header can the web server configure to instruct the user's browser to never load the site over plain HTTP again?


5. **Why can't an attacker who captures an encrypted HTTPS session simply decrypt the data using the server's publicly available digital certificate?**
