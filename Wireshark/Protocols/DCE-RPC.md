DCE/RPC stands for Distributed Computing Environment / Remote Procedure Call.
It is an advanced networking technology that allows a program running on one computer to execute code or run a program on a completely different computer as if it were running locally, without the developer having to worry about the underlying network details.
In Windows networks and Active Directory, DCE/RPC is the core engine that allows administrative tools and background services to talk to each other and pass commands between machines.
------------------------------
## The "Restaurant Kitchen" Analogy
To understand DCE/RPC easily, imagine a restaurant:

* Without RPC: A waiter (your local computer) who wants a dish made must go into the kitchen, grab the ingredients, turn on the stove, and cook the meal himself.
* With RPC: The waiter simply stands at the counter and shouts an order to the chef: "Make one order of pasta!" (Executes a Remote Procedure). The chef in the kitchen handles all the complex cooking steps, packages the meal, and hands it back to the waiter.

Your computer didn't do the hard work; it just asked a remote system to perform a specific function and send back the result.
------------------------------
## How DCE/RPC and SMB2 Work Together
If you look closely at Windows network traffic in Wireshark, you will very often see DCE/RPC traveling inside SMB2 packets. They are tightly linked partners.

* SMB2 is the Delivery Truck: It handles the file sharing, the network connection, and opens the communication pipeline between the computers.
* DCE/RPC is the Cargo: It rides inside the SMB2 pipeline to carry the actual command or programming function that needs to be executed on the server.

For example, when a network administrator remotely resets a user's password or creates a new user account, the management tool wraps a DCE/RPC command inside an SMB2 packet to tell the Active Directory Domain Controller to change the password database.
## Key Components of DCE/RPC
When analyzing this traffic, you will often come across three key concepts:

* UUID (Universally Unique Identifier): Because a server might have hundreds of functions it can perform, every unique program or service has a specific ID code (UUID). The client uses this ID to say, "I want to talk specifically to the Remote Registry service."
* Endpoint: This is the network address or named pipe where the service is listening for requests.
* EPM (Endpoint Mapper): Think of this as the building receptionist. When a client connects via Port 135, it asks the Endpoint Mapper: "Where is the print spooler service listening?" The EPM looks up the current dynamic port and points the client to the correct location.

## The Cybersecurity Risk
Because DCE/RPC allows one computer to run commands on another, hackers love to target it.

* Lateral Movement: Attackers use tools like Impacket (psexec or wmiexec) to send malicious DCE/RPC commands across a network. Once they steal administrator credentials, they use RPC to remotely install malware, start malicious services, or execute code on clean servers.

Are you seeing DCE/RPC traffic over port 135 or port 445 in your current investigation, or are you trying to understand how malware uses it to move laterally across the network?

