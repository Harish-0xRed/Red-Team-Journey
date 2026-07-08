# 📘 HTTP Cookies - Learning Notes

**Date:** 08-07-2026

---

# What are Cookies?

A **cookie** is a small piece of data stored by a web browser on behalf of a website.

Cookies allow websites to **remember information** about a user across multiple HTTP requests.

Since **HTTP is stateless**, cookies help maintain user sessions and preferences.

---

# Why are Cookies Important?

Without cookies, websites would forget every user after each request.

Cookies are commonly used for:

- User authentication
- Session management
- Remember Me functionality
- Shopping carts
- User preferences
- Website personalization
- Tracking returning visitors

---

# Why Does HTTP Need Cookies?

HTTP is a **stateless protocol**, meaning each request is treated independently.

Example:

```
Request 1
GET /login

↓

Server
"Who are you?"

-------------------

Request 2
GET /profile

↓

Server
"Who are you?"

-------------------

Request 3
GET /cart

↓

Server
"Who are you?"
```

Without cookies, users would need to log in on every page.

Cookies solve this problem by allowing the browser to identify itself with each request.

---

# Cookie Workflow

```
Browser
    │
    │ GET /
    ▼
Server

↓

Returns Login Page

↓

User Logs In

↓

POST /login

↓

Server Verifies User

↓

Set-Cookie: session=ABC123

↓

Browser Stores Cookie

↓

Future Requests

↓

Cookie: session=ABC123

↓

Server Recognizes User
```

---

# Set-Cookie Header

```http
Set-Cookie: session=ABC123
```

### Purpose

Sent by the **server**.

Instructs the browser to store a cookie.

Usually sent after:

- Successful login
- User registration
- Session creation

---

# Cookie Header

```http
Cookie: session=ABC123
```

### Purpose

Sent by the **browser**.

Returns the stored cookie back to the server with every future request.

This allows the server to identify the user.

---

# Set-Cookie vs Cookie

| Header | Direction | Purpose |
|----------|-----------|---------|
| Set-Cookie | Server → Browser | Creates and stores a cookie |
| Cookie | Browser → Server | Sends stored cookie back |

---

# Example Login Flow

### Step 1

Browser requests login page.

```http
GET /login HTTP/1.1
```

---

### Step 2

User submits credentials.

```http
POST /login HTTP/1.1

username=harish
password=********
```

---

### Step 3

Server verifies credentials.

Server responds:

```http
HTTP/1.1 200 OK

Set-Cookie: session=ABC123
```

Browser stores:

```
session=ABC123
```

---

### Step 4

User visits another page.

Browser automatically sends:

```http
GET /profile HTTP/1.1

Cookie: session=ABC123
```

Server identifies the session and grants access.

---

# Session Tokens

Modern websites **do not store passwords in cookies**.

Instead, they store a **session token**.

Example:

```http
Cookie: session=92AF81D731CCB8
```

The server maps the session token to the logged-in user.

Benefits:

- Password isn't repeatedly transmitted.
- Sessions can expire or be revoked.
- More secure than storing credentials.

---

# Common Uses of Cookies

- User login sessions
- Shopping cart contents
- Language preferences
- Theme settings (Dark/Light Mode)
- Remember Me functionality
- Analytics and tracking

---

# Cookie Flow Diagram

```
Server
   │
   │ Set-Cookie
   ▼
Browser

Stores Cookie

↓

Future Request

↓

Cookie Header

↓

Server

Recognizes User
```

---

# Cybersecurity Notes

Cookies are one of the most important components in web security.

Security analysts often inspect cookies during:

- Burp Suite testing
- API testing
- Session management analysis
- Authentication testing

Important points:

- Cookies usually contain **session IDs**, not passwords.
- Stolen session cookies can allow attackers to impersonate users (Session Hijacking).
- Sensitive cookies should use:
  - Secure
  - HttpOnly
  - SameSite

These attributes help protect cookies from theft and misuse.

---

# Interview Points

**Q:** What is a cookie?

> A small piece of data stored by the browser that helps websites remember users between HTTP requests.

---

**Q:** Why are cookies needed?

> Because HTTP is stateless and doesn't remember previous requests.

---

**Q:** What is the difference between Cookie and Set-Cookie?

- **Set-Cookie** → Server tells the browser to store a cookie.
- **Cookie** → Browser sends the stored cookie back to the server.

---

**Q:** Does a cookie normally store the user's password?

> No. Modern websites typically store a session identifier or token instead of the user's password.

---

# Quick Revision

```
HTTP

↓

Stateless

↓

Needs Memory

↓

Cookies

↓

Server
Set-Cookie

↓

Browser Stores Cookie

↓

Future Requests

↓

Cookie

↓

Server Recognizes User
```

---

# Key Takeaways

- Cookies store small pieces of website data in the browser.
- They help maintain user sessions across multiple HTTP requests.
- **Set-Cookie** creates a cookie; **Cookie** returns it to the server.
- Modern applications use session tokens instead of storing passwords.
- Cookies are fundamental to web authentication and session management.

---
