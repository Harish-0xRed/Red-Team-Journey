📘 HTTP Status Codes - Learning Notes

Date: 08-07-2026

What are HTTP Status Codes?

HTTP Status Codes are 3-digit numbers returned by a web server in an HTTP response.

They tell the client (browser or application) whether the request was successful or if an error occurred.

Every HTTP response begins with a status line containing the HTTP version and a status code.

Example:

HTTP/1.1 200 OK
Why are HTTP Status Codes Important?

Status codes help clients understand the result of their request.

They are used for:

Web browsing
REST APIs
Debugging applications
Troubleshooting servers
SOC monitoring
Penetration testing
Status Code Categories

HTTP status codes are divided into 5 categories.

Range	Category	Meaning
100–199	Informational	Request received, continue
200–299	Success	Request completed successfully
300–399	Redirection	Resource has moved
400–499	Client Error	Problem with the client's request
500–599	Server Error	Problem on the server
1xx - Informational

The server has received the request and expects the client to continue.

Example:

100 Continue

Common Use:

Large file uploads
Continue sending request body
2xx - Success

The request completed successfully.

200 OK
HTTP/1.1 200 OK

Meaning:

Request successful
Resource returned correctly

Example:

GET /profile

↓

Profile page loads successfully.

201 Created
HTTP/1.1 201 Created

Meaning:

A new resource has been created.

Example:

POST /users

↓

New user account created.

3xx - Redirection

The requested resource has moved.

301 Moved Permanently
301 Moved Permanently

Meaning:

The resource has permanently moved to another URL.

Example:

http://example.com

↓

https://example.com
302 Found
302 Found

Meaning:

Temporary redirect.

The original URL may be used again later.

4xx - Client Errors

The client sent an invalid request.

400 Bad Request

Meaning:

The request is malformed or missing required information.

Example:

Missing required parameters in a form submission.

401 Unauthorized

Meaning:

Authentication is required.

Example:

Trying to access:

/admin

without logging in.

403 Forbidden

Meaning:

The client is authenticated but doesn't have permission.

Example:

Normal user trying to access an administrator page.

404 Not Found

Meaning:

Requested page or resource does not exist.

Example:

GET /unknown-page
405 Method Not Allowed

Meaning:

The requested HTTP method is not supported.

Example:

Sending:

GET /create-account

when the endpoint only accepts:

POST /create-account
5xx - Server Errors

The request reached the server, but the server failed while processing it.

500 Internal Server Error

Meaning:

Unexpected server-side error.

Examples:

Application bug
Database failure
Server exception
503 Service Unavailable

Meaning:

The server is temporarily unavailable.

Common reasons:

Scheduled maintenance
High traffic
Server overload
Common Status Codes Summary
Status Code	Meaning
200	OK
201	Created
301	Moved Permanently
302	Found (Temporary Redirect)
400	Bad Request
401	Unauthorized
403	Forbidden
404	Not Found
405	Method Not Allowed
500	Internal Server Error
503	Service Unavailable
Status Code Flow
Client Sends Request
        │
        ▼
Server Processes Request
        │
        ▼
Returns Status Code

2xx → Success ✅

3xx → Redirect 🔄

4xx → Client Mistake ❌

5xx → Server Problem ⚠️
401 vs 403
401 Unauthorized	403 Forbidden
Authentication required	Authentication succeeded
User is not logged in	User is logged in
Login first	Permission denied
500 vs 503
500 Internal Server Error	503 Service Unavailable
Unexpected server failure	Temporary outage
Application bug	Maintenance or overload
Usually unplanned	Usually temporary
301 vs 302
301	302
Permanent redirect	Temporary redirect
New URL should always be used	Original URL may return later
Cybersecurity Notes

HTTP status codes provide valuable information during security analysis.

200 OK → Resource exists and is accessible.
301/302 → Redirects may reveal moved resources or HTTPS enforcement.
401 → Authentication is required.
403 → Resource exists but access is denied.
404 → Resource not found or intentionally hidden.
405 → Wrong HTTP method used; useful during API testing.
500 → Unexpected server errors may indicate application bugs.
503 → Server temporarily unavailable due to maintenance or heavy load.

SOC analysts frequently review HTTP status codes in web server logs to detect abnormal activity, failed access attempts, and application issues.

Interview Points

Q: What are HTTP status codes?

Three-digit response codes returned by the server to indicate the result of an HTTP request.

Q: Which status code indicates success?

200 OK

Q: What is the difference between 401 and 403?

401 requires authentication, while 403 means the user is authenticated but lacks permission.

Q: What is the difference between 500 and 503?

500 indicates an unexpected server error, while 503 indicates the server is temporarily unavailable.

Quick Revision
1xx → Information

2xx → Success
200 OK
201 Created

3xx → Redirect
301 Permanent
302 Temporary

4xx → Client Error
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
405 Method Not Allowed

5xx → Server Error
500 Internal Server Error
503 Service Unavailable
Key Takeaways
HTTP Status Codes indicate the outcome of an HTTP request.
The first digit identifies the response category.
2xx means success, 4xx indicates client errors, and 5xx indicates server errors.
Understanding status codes is essential for troubleshooting, API development, SOC analysis, and penetration testing.
