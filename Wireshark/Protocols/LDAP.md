Lightweight Directory Access Protocol (LDAP) is an open, industry-standard language that computers use to look up information about people, groups, and devices within an organization.
If Active Directory is like the company’s massive corporate phone book database, LDAP is the language you use to search that phone book.
------------------------------
## The Main Goal: Centralized Management
Imagine a large company with 5,000 employees. Every employee needs access to email, the corporate HR portal, the office Wi-Fi, the printers, and file shares.

* Without LDAP: The IT team would have to create a separate username and password for every employee on every single system. If an employee changes their password or leaves the company, IT would have to manually update it dozens of times.
* With LDAP: All user accounts are stored in one central place (the Directory). When you log into the corporate printer, the printer uses LDAP to ask the central server: "Hey, does John Doe work here, and is his password correct?"

------------------------------
## The Structure: An Upside-Down Tree
LDAP does not store data in flat tables like an Excel sheet. Instead, it organizes data like an upside-down tree hierarchy (a directory tree). This structure makes searching incredibly fast.

                  [ Root: dc=company, dc=com ]
                               |
               +---------------+---------------+

               |                               |
       [ ou=Employees ]                  [ ou=Devices ]

               |                               |
       +-------+-------+                       +-------+

       |               |                               |
  [ cn=Alice ]    [ cn=Bob ]                     [ cn=Printer01 ]

To find a specific user, LDAP uses a unique path called a Distinguished Name (DN). It reads from the bottom up, like an address:

* cn=Alice,ou=Employees,dc=company,dc=com
(Translation: Find the common name Alice, inside the organizational unit Employees, at the domain company.com).

## Key Terms Decoded

* dc (Domain Component): Breaks down the internet domain name (e.g., company.com becomes dc=company,dc=com).
* ou (Organizational Unit): Think of this like a folder used to group things (e.g., Employees, Finance, Computers).
* cn (Common Name): The individual object itself (e.g., a person's name or a printer's model name).

------------------------------
## The 4 Basic Steps of how LDAP Works
When a computer wants to talk to an LDAP server, the conversation follows a very structured pattern:

   1. Bind (Log In): The client computer connects to the LDAP server and authenticates itself (usually with a service account username and password).
   2. Search / Query: The client asks a specific question. For example: "Search the 'Employees' folder and give me the email address for anyone whose job title is 'Manager'."
   3. Response: The LDAP server instantly sends back the matching results.
   4. Unbind (Log Out): The client closes the connection.

------------------------------
## Security Alert: LDAP vs. LDAPS
By default, standard LDAP communicates over Port 389 in plain text. This means if a hacker uses a tool like Wireshark on your network, they can easily read the corporate usernames and passwords passing by.
Because of this, modern security operations centers (SOCs) require LDAPS (LDAP over SSL/TLS), which runs over Port 636. LDAPS completely encrypts the traffic, keeping the corporate phone book and login credentials hidden from network snoopers.
Are you seeing LDAP traffic in your Wireshark logs while investigating the infected host, or are you trying to understand how Active Directory uses LDAP to authenticate users?

