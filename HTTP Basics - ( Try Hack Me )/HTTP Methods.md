📘 HTTP Methods - Learning Notes

Date: 08-07-2026

What are HTTP Methods?

HTTP Methods (also called HTTP Verbs) define what action the client wants the server to perform on a resource.

Whenever a browser or application sends an HTTP request, it includes a method that tells the server the intended operation.

Why are HTTP Methods Important?

They allow clients to:

Retrieve data
Submit new data
Update existing data
Delete resources

HTTP methods are widely used in:

Web applications
REST APIs
Mobile applications
Cloud services
CRUD Mapping

HTTP methods follow the CRUD (Create, Read, Update, Delete) model.

CRUD Operation	HTTP Method	Purpose
Create	POST	Create a new resource
Read	GET	Retrieve data
Update	PUT	Update an existing resource
Delete	DELETE	Remove a resource
1. GET Method
GET /products HTTP/1.1
Purpose

Retrieve information from the server.

Common Uses
Open webpages
View user profiles
Read product details
Fetch API data
Characteristics
Retrieves data only
Does not normally modify server data
Can be repeated safely
2. POST Method
POST /users HTTP/1.1
Purpose

Send data to the server to create a new resource.

Common Uses
User Registration
Login Forms
Contact Forms
File Uploads
Characteristics
Creates new resources
Usually contains a request body
Changes server-side data
3. PUT Method
PUT /users/15 HTTP/1.1
Purpose

Update or replace an existing resource.

Common Uses
Edit user profile
Update product details
Modify account information
Characteristics
Updates existing data
Requires the target resource
4. DELETE Method
DELETE /users/15 HTTP/1.1
Purpose

Delete an existing resource.

Common Uses
Delete user account
Remove products
Delete blog posts
Characteristics
Removes existing data
Requires proper authorization
HTTP Methods Flow
Client
   │
   ├── GET      → Read Data
   ├── POST     → Create Data
   ├── PUT      → Update Data
   └── DELETE   → Remove Data
            │
            ▼
        Web Server
            │
            ▼
       HTTP Response
Real-World Examples
View Products
GET /products

↓

Returns product list.

Create Product
POST /products

↓

Creates a new product.

Update Product
PUT /products/25

↓

Updates Product ID 25.

Delete Product
DELETE /products/25

↓

Deletes Product ID 25.

HTTP Methods Comparison
Method	Purpose	Changes Server Data?	Request Body
GET	Retrieve data	No	Usually No
POST	Create resource	Yes	Yes
PUT	Update resource	Yes	Yes
DELETE	Remove resource	Yes	Usually No
Example Workflow
User Opens Website
        │
        ▼
GET /products

↓

View Products

↓

POST /products

↓

Create Product

↓

PUT /products/25

↓

Update Product

↓

DELETE /products/25

↓

Remove Product
Cybersecurity Notes

Understanding HTTP methods is essential for:

REST API Security
Burp Suite Testing
API Penetration Testing
Web Application Security
OWASP Top 10
SOC Log Analysis

Examples:

Unexpected DELETE requests may indicate unauthorized deletion attempts.
POST requests often contain login credentials, uploaded files, or form data.
PUT requests modify existing resources and should require proper authorization.
Analysts inspect HTTP methods in logs to understand user activity and identify suspicious behavior.
Interview Points

Q: Which HTTP method retrieves information?

GET

Q: Which HTTP method creates new resources?

POST

Q: Which HTTP method updates existing resources?

PUT

Q: Which HTTP method deletes resources?

DELETE

Q: What does CRUD stand for?

Create, Read, Update, Delete

Quick Revision
GET
↓

Read Data

----------------

POST
↓

Create Data

----------------

PUT
↓

Update Data

----------------

DELETE
↓

Delete Data
Key Takeaways
HTTP Methods define the action a client wants the server to perform.
GET retrieves data without normally changing server state.
POST creates new resources.
PUT updates existing resources.
DELETE removes resources.
These methods form the foundation of REST APIs and web application communication.
