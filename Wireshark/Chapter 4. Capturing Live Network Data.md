````markdown
# Wireshark Chapter 4 — Capturing Live Network Data

> Source: Wireshark User's Guide — Chapter 4
> Topic: Capturing Live Network Data
> Purpose: Understand how Wireshark captures packets before learning how to deeply analyze them.

---

# 1. What Is Live Packet Capture?

Live packet capture means:

> Wireshark observes network traffic as it is happening and records packets for analysis.

Instead of opening an old `.pcap` file, Wireshark connects to a capture interface and receives packets from the system's packet-capture engine.

Basic flow:

```text
Network Traffic
      ↓
Network Interface
      ↓
libpcap / capture engine
      ↓
Wireshark
      ↓
Capture Filter
      ↓
Captured Packets
      ↓
Capture File
      ↓
Packet Analysis
````

Wireshark's capture engine is one of its major features.

It can capture traffic from different kinds of network hardware, including:

* Ethernet
* 802.11 / Wi-Fi
* USB
* Bluetooth
* Other supported packet sources

It can also capture from multiple interfaces at the same time.

---

# 2. What Wireshark Can Do During Capture

Wireshark can:

* Capture packets from different network interfaces.
* Capture from multiple interfaces simultaneously.
* Display decoded packets while capture is running.
* Stop capture based on conditions such as:

  * amount of captured data
  * elapsed time
  * number of packets
* Apply capture filters.
* Save long-term captures into multiple files.
* Use a ring buffer so old capture files are replaced by newer ones.

Important:

Wireshark can filter packets while capturing, but it does not provide a general mechanism to stop the capture based on the contents of a captured packet.

For example:

```text
Possible:

Stop after:
    1 GB of data
    10,000 packets
    10 minutes

Not directly supported:

"Stop when HTTP packet contains this specific data"
```

---

# 3. Capture Prerequisites

Before starting a capture, three things are especially important:

## 3.1 Correct privileges

Depending on the operating system and configuration, capturing packets may require special privileges.

For example:

```text
Normal user
     ↓
Wireshark
     ↓
Capture interface
     ↓
Permission required?
```

If Wireshark cannot access the interface, the capture may fail.

---

# 3.2 Choose the Correct Interface

Your computer can have multiple interfaces.

Example:

```text
Ethernet
Wi-Fi
VPN
Loopback
Bluetooth
USB
```

If you want to capture Wi-Fi traffic but select Ethernet, you won't see the traffic you expected.

Therefore:

> The interface you select determines where Wireshark gets its packets.

---

# 3.3 Capture at the Correct Network Location

This is extremely important.

Even if Wireshark is configured correctly, you cannot automatically see traffic that never reaches the interface where you are capturing.

Example:

```text
PC A
  |
  |
Switch
  |
  |
PC B
```

If your Wireshark capture point is somewhere that does not receive the traffic between A and B, Wireshark cannot magically see it.

Think:

```text
Correct interface
        +
Correct network location
        =
Traffic you can capture
```

---

# 4. Starting a Capture

There are several ways to start a capture.

## Method 1 — Double-click an interface

From the Wireshark Welcome Screen:

```text
Select interface
       ↓
Double-click
       ↓
Capture starts
```

---

## Method 2 — Select interface + Capture → Start

You can:

```text
Select interface
       ↓
Capture → Start
```

or use the toolbar button.

---

## Method 3 — Capture Options

You can open:

```text
Capture → Options
```

This provides much more control over the capture.

You can configure:

* interface
* promiscuous mode
* snapshot length
* buffer
* monitor mode
* capture filter
* output file
* file rotation
* ring buffer
* real-time display
* name resolution
* automatic stop conditions

---

## Method 4 — Command Line

If the interface name is known:

```bash
wireshark -i eth0 -k
```

Meaning:

```text
wireshark
   ↓
Start Wireshark

-i eth0
   ↓
Use eth0 as capture interface

-k
   ↓
Start capturing immediately
```

---

# 5. Wireshark Welcome Screen — Capture Section

When Wireshark starts without opening a capture file or starting a capture, it shows the Welcome Screen.

The Capture section lists available capture interfaces.

Example:

```text
Capture
────────────────────────

Ethernet
Wi-Fi
Loopback
VPN
Bluetooth
```

Wireshark also shows network activity for interfaces using a small activity graph called a:

> Sparkline

The sparkline gives a quick visual indication of traffic activity.

---

# 6. Multiple Interface Capture

Wireshark can capture from multiple interfaces simultaneously.

Example:

```text
Wi-Fi ───────┐
             │
Ethernet ────┼──→ Wireshark
             │
VPN ─────────┘
```

This can be useful when traffic exists across multiple interfaces.

For example:

```text
Wi-Fi traffic
+
VPN traffic
```

can be captured together if the interfaces are selected.

When multiple interfaces are used, `pcapng` may be required because it supports more flexible capture information than traditional `pcap`.

---

# 7. Interface Configuration Icon

Some interfaces have a configuration icon beside them.

This means the interface has additional configuration options.

Clicking the icon opens the configuration dialog for that interface.

---

# 8. Interface Information

Hovering over an interface can show information such as:

```text
IPv4 address
IPv6 address
Capture filter
```

This can help identify whether you are selecting the interface you actually want.

---

# 9. Wireshark Is Not Limited to Normal Network Interfaces

Wireshark can capture more than just Ethernet and Wi-Fi.

Depending on the operating system and available capture mechanisms, it can also capture things such as:

* USB
* Bluetooth
* other supported packet sources

An interface may also be hidden if:

* Wireshark cannot access it
* it has been configured to be hidden

---

# 10. Capture Options Dialog

Open:

```text
Capture → Options
```

The Capture Options dialog provides detailed control over packet capture.

It has several important areas:

```text
Input
Output
Options
```

---

# 11. Input Tab

The Input tab contains the interface table.

Important columns include:

```text
Interface
Traffic
Link-layer Header
Promiscuous
Snaplen
Buffer
Monitor Mode
Optimize
Capture Filter
```

Let's understand each one.

---

# 12. Interface

This is the name of the interface Wireshark will capture from.

Example:

```text
eth0
wlan0
Wi-Fi
Ethernet
```

The exact names depend on the operating system.

---

# 13. Traffic

Wireshark displays a small traffic activity graph.

This helps you visually identify which interfaces are currently active.

Example:

```text
Ethernet   ▁▂▃▅▇▆▅
Wi-Fi      ▁▁▂▂▃▂
VPN        ▁▁▁▁▁▁
```

An active interface normally shows changing activity.

---

# 14. Link-layer Header

The link-layer header tells Wireshark what kind of packet/frame representation it is receiving at the link layer.

For normal captures you usually do not need to change this.

Example:

```text
Ethernet
802.11
DOCSIS
PPP
Cisco HDLC
```

More details are covered later in Section 4.9.

---

# 15. Promiscuous Mode

Promiscuous mode allows a network interface to receive packets that are not addressed specifically to the local host, subject to what the network and capture point actually make visible.

Important:

> Promiscuous mode does NOT magically make all network traffic visible.

Think:

```text
Promiscuous Mode
        ↓
Allows interface to accept
more frames it can actually see
        ↓
Wireshark can capture them
```

But:

```text
Traffic never reaches interface
        ↓
Promiscuous mode cannot help
```

Another application may also override the promiscuous mode setting.

---

# 16. Enable Promiscuous Mode on All Interfaces

Wireshark provides an option to enable promiscuous mode on all selected interfaces.

If:

```text
Enable promiscuous mode on all interfaces
```

is enabled, it overrides the individual promiscuous-mode settings.

---

# 17. Snapshot Length — Snaplen

Snapshot length determines how many bytes of each packet are captured.

Example:

```text
Actual packet:
1500 bytes

Snaplen:
500 bytes
```

Captured:

```text
First 500 bytes
```

The rest is not captured.

Why would you use a smaller snapshot length?

Possible reasons:

* performance
* reducing capture size
* privacy

Example:

```text
Full packet
┌──────────────────────────────┐
│ Headers │ Data │ More Data   │
└──────────────────────────────┘
             ↓
          Snaplen
             ↓
┌────────────────┐
│ First N bytes  │
└────────────────┘
```

Important:

> Snaplen controls how many bytes of each packet are captured.

---

# 18. Capture Buffer

The capture buffer is kernel memory reserved for incoming captured packets.

Basic flow:

```text
Network
   ↓
Network Interface
   ↓
Kernel Capture Buffer
   ↓
Wireshark
```

The buffer provides temporary storage for captured packets while they are being processed.

If the system receives traffic faster than it can process, buffer capacity becomes important.

The default is usually sufficient.

---

# 19. Monitor Mode

Monitor mode is mainly relevant to Wi-Fi / 802.11 capture.

It allows capture of full, raw 802.11 headers when supported by:

* hardware
* driver
* operating system
* interface type

Important warning:

> Enabling monitor mode may disconnect you from the wireless network.

Normal Wi-Fi operation:

```text
Wi-Fi interface
      ↓
Normal wireless connection
```

Monitor mode:

```text
Wi-Fi interface
      ↓
Raw 802.11 capture
```

Support varies by platform.

---

# 20. Optimize

Wireshark normally optimizes the capture filter when compiling it into bytecode.

This optimization should generally remain enabled.

Why?

Because optimized filter code can improve execution.

You might disable it when:

* investigating a possible optimizer bug
* learning how the compiled filter works
* educational purposes

Normal rule:

> Leave Optimize enabled unless you have a specific reason not to.

---

# 21. Capture Filter

A capture filter tells Wireshark which packets should be captured.

Example:

```text
tcp port 443
```

Conceptually:

```text
Network traffic
      ↓
Capture Filter
      ↓
Matching packets → captured
Non-matching → ignored
```

This is different from a display filter.

Remember:

```text
Capture Filter
    ↓
Controls what gets captured

Display Filter
    ↓
Controls what you see after capture
```

Capture filters are discussed deeply in Section 4.10.

---

# 22. Capture Filter for Multiple Interfaces

Wireshark allows:

```text
Capture filter for selected interfaces
```

This allows one filter to be applied to multiple selected interfaces.

---

# 23. Manage Interfaces

The:

```text
Manage Interfaces
```

button opens the interface management dialog.

It allows management of:

* local interfaces
* named pipes
* remote interfaces

---

# 24. Output Tab

The Output tab controls how captured packets are saved.

Important settings include:

```text
Capture to a permanent file
File
Output format
Create a new file automatically
Ring buffer
```

---

# 25. Temporary Capture File

If you leave the file field empty, Wireshark stores the capture in a temporary file.

Conceptually:

```text
Capture
   ↓
Temporary file
   ↓
Stop capture
   ↓
Save later if needed
```

This is the default behavior.

---

# 26. Permanent Capture File

You can specify a filename.

Example:

```text
incident.pcapng
```

Then Wireshark writes the capture to that file.

This is useful when you already know you want to save the capture directly.

---

# 27. Capture File Format

Wireshark commonly uses:

```text
pcapng
```

as the default capture format.

`pcapng` is more flexible than traditional `pcap`.

It can support information such as captures involving multiple interfaces.

Basic distinction:

```text
pcap
    ↓
Traditional capture format

pcapng
    ↓
More flexible modern capture format
```

---

# 28. Automatically Create New Capture Files

Wireshark can automatically switch to a new file based on conditions.

Possible conditions include:

```text
Number of packets
File size
Duration
Wall-clock time
```

Example:

```text
capture_001.pcapng
       ↓
reaches size limit
       ↓
capture_002.pcapng
       ↓
reaches size limit
       ↓
capture_003.pcapng
```

This is especially useful for long-running captures.

---

# 29. Why Multiple Capture Files?

Very large capture files can become difficult and slow to work with.

For example:

```text
One huge file
     ↓
500 MB+
     ↓
Harder to handle
```

Instead:

```text
capture_001
capture_002
capture_003
capture_004
```

Smaller files are often easier to work with.

---

# 30. Important Disadvantage of Multiple Files

There is an important trade-off.

Wireshark maintains protocol context based on the packets loaded in the current capture file.

Example:

```text
TCP connection establishment
        ↓
File 1

Later TCP traffic
        ↓
File 2
```

If you only load File 2, Wireshark may not have all the context from File 1.

Therefore:

> Splitting a capture into multiple files can reduce context when analyzing only one file.

This can affect things such as:

* stream context
* protocol establishment
* relationships between earlier and later packets

---

# 31. Ring Buffer

A ring buffer uses a fixed number of capture files.

Example:

```text
Ring buffer = 3 files

File 1
File 2
File 3
```

When File 3 is full:

```text
New capture
    ↓
File 1 is replaced
```

Then:

```text
File 2
File 3
File 4
```

The oldest file is replaced.

---

# 32. Ring Buffer Mental Model

Think of it as a rotating storage system:

```text
       ┌───────────┐
       │ File 1    │
       └─────┬─────┘
             ↓
       ┌───────────┐
       │ File 2    │
       └─────┬─────┘
             ↓
       ┌───────────┐
       │ File 3    │
       └─────┬─────┘
             ↓
          back to
          File 1
```

The oldest data gets replaced.

---

# 33. Why Use a Ring Buffer?

Ring buffers are useful for long-term monitoring.

Suppose you want:

> Always keep the latest 3 GB of traffic.

A ring buffer can continuously capture while limiting disk usage.

Concept:

```text
Capture forever
      ↓
Keep only latest files
      ↓
Oldest files replaced
```

Therefore:

> Ring buffer limits disk usage while keeping the newest capture data.

---

# 34. File Modes

Wireshark has four important capture file modes.

## 34.1 Single Temporary File

Default mode.

```text
Capture
   ↓
Temporary file
```

You can save it later.

---

## 34.2 Single Named File

```text
Capture
   ↓
incident.pcapng
```

One specified file is used.

---

## 34.3 Multiple Files — Continuous

Example:

```text
capture_00001
capture_00002
capture_00003
capture_00004
...
```

When a switching condition is reached, Wireshark creates another file.

Old files remain.

Therefore disk usage can continue growing.

---

## 34.4 Multiple Files — Ring Buffer

Example:

```text
capture_00001
capture_00002
capture_00003
```

After the configured number of files is reached:

```text
Oldest file
    ↓
replaced
```

Disk usage remains bounded.

---

# 35. File Mode Comparison

| Mode                 |        Files | Old Data Replaced? | Typical Use            |
| -------------------- | -----------: | ------------------ | ---------------------- |
| Single temporary     |            1 | No                 | Normal short capture   |
| Single named         |            1 | No                 | Specific saved capture |
| Multiple continuous  |         Many | No                 | Long capture           |
| Multiple ring buffer | Fixed number | Yes                | Long-term monitoring   |

---

# 36. Manage Interfaces Dialog

The Manage Interfaces dialog allows you to control which interfaces appear in Wireshark.

Local Interfaces contains:

```text
Show
Friendly Name
Interface Name
Comment
```

---

# 37. Show

Controls whether an interface is visible in:

```text
Welcome Screen
Capture Options
```

You can hide interfaces you don't normally use.

---

# 38. Friendly Name

A human-readable name for an interface.

Example:

```text
Interface Name:
eth0

Friendly Name:
Main Ethernet
```

The friendly name makes interface identification easier.

---

# 39. Interface Name

This is the actual device/interface name used by the system.

Example:

```text
eth0
wlan0
```

The exact naming depends on the operating system.

---

# 40. Comment

You can add a descriptive comment to an interface.

Example:

```text
eth0

Comment:
Lab Ethernet interface
```

This can help when multiple interfaces exist.

---

# 41. Named Pipes

The Pipes tab allows capture from a named pipe.

A pipe must already exist before it can be added.

Conceptually:

```text
Program A
   ↓
Named Pipe
   ↓
Wireshark
```

Wireshark can use the pipe as a packet source.

---

# 42. Remote Interfaces

Wireshark can capture from an interface on another machine.

Concept:

```text
Remote Machine
      ↓
Remote Capture
      ↓
Network
      ↓
Your Wireshark
```

On Microsoft Windows, Wireshark can use the Remote Packet Capture Protocol.

The required service must be running on the target machine.

---

# 43. Remote Capture Authentication

The remote interface configuration can use:

```text
Null authentication
```

or:

```text
Password authentication
```

Null authentication means no authentication.

The Wireshark documentation explicitly warns that this is not secure.

Password authentication requires credentials for the remote capture service.

---

# 44. Remote Capture Through SSH

On Linux/Unix, capture can be performed through an SSH tunnel.

Conceptually:

```text
Remote Linux machine
        ↓
      SSH
        ↓
   Your machine
        ↓
    Wireshark
```

This is useful because the packet capture happens remotely while analysis can happen locally.

---

# 45. Remote Packet Capture Protocol Default Port

The Remote Packet Capture Protocol uses:

```text
TCP/UDP port 2002
```

as its default port according to the guide.

If using this method, access to the required port must be available.

---

# 46. Compiled Filter Output

Wireshark can show the result of compiling a capture filter into BPF bytecode.

Example:

```text
Capture Filter
      ↓
Compiler
      ↓
BPF bytecode
```

The Compiled Filter Output dialog shows:

```text
Interface
     ↓
Compiled filter
```

This is mainly useful for:

* understanding capture filters
* troubleshooting
* learning BPF
* advanced packet capture work

For normal Wireshark use, you don't need to manually understand every instruction.

---

# 47. BPF — Berkeley Packet Filter

Capture filters use the libpcap filter language.

The filter is compiled into a form that can be executed by the packet capture system.

Conceptually:

```text
Human-readable filter
        ↓
libpcap filter compiler
        ↓
BPF instructions
        ↓
Packet filtering
```

---

# 48. BPF JIT

On Linux, BPF execution can be accelerated using:

```text
BPF Just-In-Time compilation
```

The guide provides:

```bash
echo 1 >/proc/sys/net/core/bpf_jit_enable
```

This is a power-user optimization.

For normal Wireshark learning:

> You do not need to change this.

Remember the concept rather than the command.

---

# 49. Link-Layer Header Type

The link-layer header describes the outermost link-layer representation of captured data.

Normally:

> You do not need to modify it.

Wireshark usually knows the correct link-layer type for the interface.

---

# 50. Ethernet Example

You might be offered:

```text
Ethernet
DOCSIS
```

If capturing normal Ethernet traffic:

```text
Ethernet
```

If capturing DOCSIS traffic placed onto Ethernet by an appropriate Cisco cable modem termination system:

```text
DOCSIS
```

---

# 51. 802.11 Example

On some BSD systems, an 802.11 device may provide a choice between:

```text
Ethernet
802.11
```

Ethernet mode may provide fake/cooked Ethernet headers.

802.11 mode provides full IEEE 802.11 headers.

If your analysis requires actual wireless headers:

```text
802.11
```

is generally the appropriate choice when supported.

---

# 52. Other Link-Layer Examples

The guide also describes cases such as:

```text
PPP over serial
Cisco HDLC
```

and:

```text
RFC 1483 IP-over-ATM
Sun raw ATM
```

These are specialized environments.

You don't normally need them for standard Ethernet/Wi-Fi learning.

Core concept:

> Link-layer header type tells Wireshark how to interpret the outer frame representation.

---

# 53. Capture Filtering

Capture filters are one of the most important concepts in this chapter.

A capture filter limits what Wireshark actually captures.

Example:

```text
tcp port 443
```

Concept:

```text
All network packets
        ↓
Capture Filter
        ↓
Only matching packets captured
```

---

# 54. Capture Filter Language

Wireshark capture filters use:

> libpcap filter language

The syntax is based around primitives combined with:

```text
and
or
not
```

General structure:

```text
[not] primitive [and|or [not] primitive ...]
```

---

# 55. Capture Filter Example

```text
tcp port 23 and host 10.0.0.5
```

Meaning:

```text
TCP
AND
port 23
AND
host 10.0.0.5
```

So the filter selects Telnet traffic involving that host.

---

# 56. Another Capture Filter Example

```text
tcp port 23 and not src host 10.0.0.5
```

Meaning:

```text
TCP port 23
AND
source host is NOT 10.0.0.5
```

This excludes traffic whose source is that host.

---

# 57. Primitive — host

Syntax:

```text
host <host>
```

Matches packets where the host appears as:

```text
source
OR
destination
```

Example:

```text
host 192.168.1.10
```

Meaning:

```text
Source = 192.168.1.10
OR
Destination = 192.168.1.10
```

---

# 58. src host

Syntax:

```text
src host <host>
```

Matches only when the host is the source.

Example:

```text
src host 192.168.1.10
```

Meaning:

```text
Source IP = 192.168.1.10
```

---

# 59. dst host

Syntax:

```text
dst host <host>
```

Matches only when the host is the destination.

Example:

```text
dst host 192.168.1.10
```

Meaning:

```text
Destination IP = 192.168.1.10
```

---

# 60. Ethernet Host Filtering

You can filter Ethernet addresses using:

```text
ether host
```

Example:

```text
ether host 00:11:22:33:44:55
```

This checks Ethernet/MAC addresses.

You can also specify:

```text
ether src host
```

or:

```text
ether dst host
```

---

# 61. IP Address vs MAC Address

Remember:

```text
host
   ↓
IP address / host

ether host
   ↓
Ethernet/MAC address
```

Example:

```text
host 192.168.1.10
```

vs.

```text
ether host 00:11:22:33:44:55
```

---

# 62. gateway host

Syntax:

```text
gateway host <host>
```

This matches packets where the specified host acted as a gateway.

Conceptually:

```text
Ethernet source/destination
        ↓
Gateway host
        ↓
But IP source/destination
is not that gateway
```

This is an advanced primitive.

For beginner/intermediate Wireshark work:

> Understand what it means, but don't prioritize memorizing it.

---

# 63. Network Filtering — net

Syntax:

```text
net <network>
```

You can also specify:

```text
src net
dst net
```

Example concept:

```text
src net 192.168.1.0/24
```

means traffic originating from that network.

---

# 64. Port Filtering

Syntax:

```text
[tcp|udp] [src|dst] port <port>
```

Example:

```text
tcp port 443
```

Meaning:

```text
TCP
+
port 443
```

---

# 65. TCP Source Port

Example:

```text
tcp src port 443
```

Means:

```text
TCP
AND
source port = 443
```

---

# 66. TCP Destination Port

Example:

```text
tcp dst port 443
```

Means:

```text
TCP
AND
destination port = 443
```

---

# 67. UDP Port Filtering

Example:

```text
udp port 53
```

This selects UDP traffic involving port 53.

Commonly associated with DNS traffic.

---

# 68. Why Specify TCP or UDP?

Port numbers alone do not completely identify the transport protocol.

For example:

```text
TCP port 53
```

and:

```text
UDP port 53
```

are different traffic.

Therefore:

```text
tcp port 443
```

is different from:

```text
udp port 443
```

---

# 69. less / greater

Capture filters can select packets based on length.

Syntax:

```text
less <length>
greater <length>
```

Conceptually:

```text
less 100
```

means packets whose length is less than or equal to the specified value.

```text
greater 1000
```

means packets whose length is greater than or equal to the specified value.

---

# 70. Protocol Filtering

You can filter by protocol using:

```text
ip proto <protocol>
```

or:

```text
ether proto <protocol>
```

This allows filtering based on protocol at the appropriate layer.

---

# 71. Broadcast and Multicast

You can filter broadcast or multicast traffic using:

```text
ip broadcast
ip multicast

ether broadcast
ether multicast
```

This is useful when studying traffic that is sent to multiple recipients.

---

# 72. Complex Byte Expressions

Capture filters can also inspect specific packet bytes or ranges.

Conceptually:

```text
Packet
   ↓
Specific byte/range
   ↓
Comparison
   ↓
Match / no match
```

These expressions are more advanced.

For normal Wireshark learning:

> Learn the common primitives first.

---

# 73. Capture Filter vs Display Filter

This distinction is extremely important.

## Capture Filter

Applied:

```text
BEFORE / DURING capture
```

It determines which packets are captured.

Example:

```text
tcp port 443
```

Packets that don't match may never enter your capture.

---

## Display Filter

Applied:

```text
AFTER packets have been captured
```

It determines which already-captured packets are displayed.

Mental model:

```text
Network
   ↓
Capture Filter
   ↓
Captured packets
   ↓
Display Filter
   ↓
Packets you currently see
```

---

# 74. Why Capture Filters Matter

Imagine:

```text
10,000,000 packets
```

but you only care about:

```text
TCP port 443
```

A capture filter can reduce the amount of data captured.

Advantages:

* smaller capture files
* less irrelevant traffic
* reduced processing
* easier analysis
* useful for long-term captures

But be careful:

> If you filter out traffic during capture, those packets are not available later in that capture file.

Therefore, capture filters should be chosen carefully.

---

# 75. Automatic Remote Traffic Filtering

When Wireshark itself is running remotely, the connection carrying the remote session generates network traffic.

Example:

```text
Your machine
     ↓
SSH
     ↓
Remote machine
     ↓
Wireshark
```

The SSH connection itself generates packets.

If Wireshark captured everything, the remote-control traffic could add a lot of noise.

---

# 76. Why Remote Traffic Filtering Exists

Imagine:

```text
Interesting traffic
        +
SSH traffic carrying Wireshark session
        +
Other remote-session traffic
```

Wireshark attempts to identify the remote connection and automatically create a capture filter matching aspects of that connection.

The goal is to reduce unnecessary remote-session traffic.

---

# 77. Remote Environment Information

Wireshark can examine environment information such as:

```text
SSH_CONNECTION
SSH_CLIENT
REMOTEHOST
DISPLAY
SESSIONNAME
```

On Windows it asks the operating system whether it is running in a Remote Desktop Services environment.

The purpose is to identify remote operation.

---

# 78. Capture While Running

Once capture begins, Wireshark can show information about the traffic while packets are being captured.

If enabled, the Capture Information dialog can display protocol activity over time.

Conceptually:

```text
Live Capture
      ↓
Protocols detected
      ↓
Activity over time
```

---

# 79. Real-Time Packet List

The Capture Options dialog has:

```text
Update list of packets in real-time
```

If enabled:

```text
Packet arrives
      ↓
Capture
      ↓
Packet appears in packet list
```

If disabled:

```text
Packet arrives
      ↓
Capture
      ↓
No packet list update during capture
      ↓
Packets displayed after capture stops
```

The real-time display option causes Wireshark to capture in a separate process and feed captures to the display process.

---

# 80. Automatic Scrolling

Wireshark can automatically scroll the packet list as new packets arrive.

Concept:

```text
Packet 1
Packet 2
Packet 3
...
Packet 100
```

The list keeps moving toward the newest packet.

If you manually scroll upward to inspect an older packet, automatic scrolling is temporarily disabled.

It resumes when you return to the end of the packet list.

---

# 81. Capture Information Dialog

The Capture Information dialog shows protocol activity while a capture is running.

It can be enabled through the relevant Wireshark setting.

Conceptually:

```text
Capture
   ↓
Protocol activity
   ↓
Live statistics
```

This can give you a quick overview of what protocols are active.

---

# 82. Name Resolution During Capture

Wireshark can resolve names during capture.

Options include:

```text
Resolve MAC addresses
Resolve network names
Resolve transport names
```

Meaning:

### MAC address resolution

Translate MAC addresses into names when possible.

### Network name resolution

Translate network addresses into names.

### Transport name resolution

Translate transport port numbers into names.

Example concept:

```text
443
 ↓
https
```

instead of simply displaying the numeric port.

---

# 83. Stop Conditions

Wireshark can automatically stop capturing based on conditions.

Examples:

```text
Number of packets
Number of capture files
Capture file size
Capture file duration
```

Example:

```text
Stop after:

100,000 packets
```

or:

```text
Stop after:

500 MB
```

or:

```text
Stop after:

30 minutes
```

---

# 84. Manual Stop

A running capture can be stopped in several ways.

### Method 1

Click:

```text
Stop Capture
```

---

### Method 2

Use:

```text
Capture → Stop
```

---

### Method 3

Use the Stop toolbar button.

---

### Method 4

Keyboard shortcut:

```text
Ctrl + E
```

---

### Method 5

Automatic stop condition.

Example:

```text
Maximum capture size reached
        ↓
Capture stops automatically
```

---

# 85. Restarting a Capture

Wireshark can restart a running capture using the same capture options.

Restart means:

```text
Current capture
     ↓
Discard captured packets
     ↓
Start a new capture
     ↓
Use same capture settings
```

This is useful if you accidentally captured a lot of irrelevant traffic and want to start cleanly.

---

# 86. Restart vs Stop + Start

Restart is essentially a convenience function.

Conceptually:

```text
Restart
   =
Stop
   +
Immediately Start Again
```

The previous packets are removed.

---

# 87. When Should You Use Restart?

Example:

You start capturing:

```text
Packet 1
Packet 2
Packet 3
...
```

Then realize:

> "I haven't started the activity I actually want to investigate."

Instead of keeping the irrelevant packets:

```text
Capture → Restart
```

Then perform the activity.

Result:

```text
Clean capture
      ↓
Only traffic generated after restart
```

---

# 88. Complete Capture Workflow

A normal Wireshark capture workflow looks like this:

```text
1. Start Wireshark
        ↓
2. Identify interfaces
        ↓
3. Select correct interface
        ↓
4. Decide whether capture filter is needed
        ↓
5. Configure capture options if necessary
        ↓
6. Start capture
        ↓
7. Generate / observe network activity
        ↓
8. Watch packets arrive
        ↓
9. Stop capture
        ↓
10. Save capture file
        ↓
11. Analyze packets
```

---

# 89. Capture Architecture

A useful mental model is:

```text
                NETWORK
                   │
                   ▼
          Network Interface
                   │
                   ▼
             libpcap
                   │
                   ▼
          Kernel Capture Buffer
                   │
                   ▼
           Capture Filtering
                   │
                   ▼
             Wireshark
             /       \
            /         \
           ▼           ▼
     Packet List    Capture File
           │
           ▼
      Later Analysis
```

---

# 90. What Happens When a Packet Arrives?

Simplified:

```text
Network packet
      ↓
Network interface receives it
      ↓
Capture mechanism receives packet
      ↓
Capture filter is evaluated
      ↓
If packet matches:
      ↓
Packet is captured
      ↓
Wireshark receives it
      ↓
Packet can be decoded/displayed
      ↓
Packet can be saved
```

If the packet does not match the capture filter:

```text
Packet
  ↓
Doesn't match capture filter
  ↓
Not captured
```

---

# 91. Important Concept — Visibility

Wireshark can only analyze packets that are available at its capture point.

This is one of the most important networking concepts to remember.

Example:

```text
             Switch
          /     |     \
         /      |      \
       PC A    PC B    PC C
```

If Wireshark is running on PC A:

```text
PC A
 ↓
Can normally capture traffic visible to PC A's interface
```

It does not automatically mean:

```text
PC A
 ↓
Can see every packet between PC B and PC C
```

The network topology and capture location matter.

---

# 92. Promiscuous Mode Does Not Equal "See Everything"

Common beginner misunderstanding:

> "If I enable promiscuous mode, Wireshark will see all network traffic."

Not necessarily.

Correct understanding:

```text
Promiscuous mode
        ↓
Allows interface to accept
more frames that reach it
```

It does NOT:

```text
Create a network tap
OR
redirect traffic
OR
make a switch send all traffic
OR
make distant traffic visible
```

---

# 93. Capture Location Matters

Suppose:

```text
PC A ── Switch ── PC B
```

Traffic:

```text
PC A → PC B
```

If the traffic never reaches your capture interface:

```text
Wireshark
   ↓
Nothing to capture
```

To observe traffic between other systems, the capture point must be somewhere the traffic actually passes.

Possible network designs may provide appropriate capture points such as:

```text
network tap
switch monitoring/mirroring
router
gateway
appropriate monitoring interface
```

The exact method depends on the network.

---

# 94. Temporary vs Permanent Capture

Think:

```text
Temporary
   ↓
Good for quick experiments

Permanent
   ↓
Good when you know you want to save the capture
```

---

# 95. Single File vs Multiple Files

Think:

```text
Small/short capture
      ↓
Single file

Long/high-volume capture
      ↓
Multiple files
```

---

# 96. Continuous vs Ring Buffer

Think:

```text
Continuous
    ↓
Keep creating files
    ↓
Disk usage grows
```

versus:

```text
Ring Buffer
    ↓
Fixed number of files
    ↓
Oldest gets replaced
    ↓
Disk usage remains bounded
```

---

# 97. Capture Filter Mental Model

Remember:

```text
Capture Filter
      ↓
Decides what enters the capture
```

Example:

```text
tcp port 443
```

means approximately:

```text
"Only capture TCP traffic involving port 443."
```

---

# 98. Important Capture Filter Primitives

| Primitive        | Meaning                                   |
| ---------------- | ----------------------------------------- |
| `host`           | Source or destination host                |
| `src host`       | Source host                               |
| `dst host`       | Destination host                          |
| `ether host`     | Ethernet/MAC host                         |
| `ether src host` | Source MAC                                |
| `ether dst host` | Destination MAC                           |
| `gateway host`   | Host acting as gateway                    |
| `net`            | Source or destination network             |
| `src net`        | Source network                            |
| `dst net`        | Destination network                       |
| `tcp port`       | TCP port                                  |
| `udp port`       | UDP port                                  |
| `src port`       | Source port                               |
| `dst port`       | Destination port                          |
| `less`           | Packet length less than/equal to value    |
| `greater`        | Packet length greater than/equal to value |
| `ip proto`       | IP protocol                               |
| `ether proto`    | Ethernet protocol                         |
| `broadcast`      | Broadcast traffic                         |
| `multicast`      | Multicast traffic                         |

---

# 99. Capture Filter Operators

Main logical operators:

```text
and
or
not
```

Examples:

```text
tcp port 443 and host 192.168.1.10
```

```text
tcp port 443 or udp port 53
```

```text
tcp port 443 and not host 192.168.1.10
```

Mental model:

```text
and
 ↓
Both conditions

or
 ↓
Either condition

not
 ↓
Exclude condition
```

---

# 100. Capture Filter Examples

## Capture HTTPS traffic

```text
tcp port 443
```

---

## Capture DNS over UDP

```text
udp port 53
```

---

## Capture traffic to/from one host

```text
host 192.168.1.10
```

---

## Capture traffic from one host

```text
src host 192.168.1.10
```

---

## Capture traffic to one host

```text
dst host 192.168.1.10
```

---

## Capture HTTPS traffic involving one host

```text
tcp port 443 and host 192.168.1.10
```

---

## Exclude traffic from one host

```text
not src host 192.168.1.10
```

---

# 101. Common Mistakes

## Mistake 1 — Choosing the wrong interface

```text
Want Wi-Fi traffic
        ↓
Select Ethernet
        ↓
"I see nothing!"
```

Always verify the interface.

---

## Mistake 2 — Thinking promiscuous mode sees everything

Wrong:

```text
Promiscuous = all network traffic
```

Correct:

```text
Promiscuous = accept more frames
that are actually visible to the interface
```

---

## Mistake 3 — Using an overly restrictive capture filter

Example:

```text
tcp port 443
```

Then later realizing:

```text
"I also needed DNS."
```

But DNS packets were never captured.

---

## Mistake 4 — Confusing capture and display filters

Capture:

```text
tcp port 443
```

Display filters are a different filtering system and are applied to captured packets.

---

## Mistake 5 — Using one enormous capture file

Long captures can create very large files.

Multiple-file capture can make large captures easier to manage.

---

## Mistake 6 — Forgetting context between files

If related packets are split between files, analyzing only one file may lose protocol context.

---

# 102. Important Settings to Understand Now

You do NOT need to memorize every option.

For practical Wireshark learning, understand these well:

```text
Interface
Promiscuous Mode
Snaplen
Buffer
Monitor Mode
Capture Filter
Output File
pcapng
Multiple Files
Ring Buffer
Stop Conditions
```

---

# 103. Settings You Can Leave Alone for Now

These are useful to recognize but don't need deep study yet:

```text
BPF JIT
Compiled BPF bytecode
gateway host
complex byte expressions
specialized link-layer types
remote packet capture protocol details
named pipes
specialized ATM/DAG configurations
```

Learn them when a practical situation requires them.

---

# 104. Chapter 4 Core Mental Model

The entire chapter can be reduced to:

```text
              NETWORK
                 │
                 ▼
        Choose Capture Point
                 │
                 ▼
          Choose Interface
                 │
                 ▼
       Configure Capture
                 │
        ┌────────┼────────┐
        │        │        │
        ▼        ▼        ▼
   Promisc.   Snaplen   Filter
        │        │        │
        └────────┼────────┘
                 ▼
             Capture
                 │
                 ▼
          Save / Display
                 │
        ┌────────┴────────┐
        ▼                 ▼
   Single File       Multiple Files
                            │
                       ┌────┴────┐
                       ▼         ▼
                  Continuous  Ring Buffer
```

---

# 105. Chapter 4 — What You Should Actually Remember

## Level 1 — Must Know

### 1. Interface

> The interface determines where Wireshark captures traffic from.

### 2. Capture point

> You can only capture traffic that reaches your capture point.

### 3. Promiscuous mode

> Allows the interface to accept more visible frames; it does not magically expose all network traffic.

### 4. Snaplen

> Controls how many bytes of each packet are captured.

### 5. Buffer

> Temporary kernel memory used for captured packets.

### 6. Monitor mode

> Wi-Fi mode that can provide raw 802.11 headers when supported.

### 7. Capture filter

> Controls which packets are captured.

### 8. Capture file

> Stores captured packet data for later analysis.

### 9. Multiple files

> Splits a capture into several files.

### 10. Ring buffer

> Reuses a fixed number of files, replacing the oldest data.

---

# 106. Level 2 — Should Understand

Understand these concepts:

```text
pcap vs pcapng
Temporary vs permanent capture
Continuous vs ring buffer
Stop conditions
Multiple interface capture
Link-layer header
Remote capture
BPF
Compiled capture filters
Automatic remote traffic filtering
```

---

# 107. Level 3 — Advanced Recognition

You don't need to master these yet:

```text
BPF bytecode
BPF JIT
gateway primitive
complex byte expressions
named pipes
Remote Packet Capture Protocol
specialized link-layer encapsulations
```

Recognize them and move on.

---

# 108. The Most Important Difference in Chapter 4

Remember this:

```text
CAPTURE
   ↓
What packets should enter my capture?
```

versus later:

```text
ANALYSIS
   ↓
What can I learn from the packets I captured?
```

Chapter 4 is primarily about:

```text
GETTING THE RIGHT PACKETS
```

Chapter 6 will focus much more heavily on:

```text
UNDERSTANDING THE PACKETS
```

---

# 109. Chapter 4 → Chapter 5 → Chapter 6

Your Wireshark learning path now looks like:

```text
Chapter 4
Capturing Live Network Data
        ↓
"How do I correctly obtain traffic?"
        ↓
Chapter 5
File Input/Output
        ↓
"How do I save, open, merge,
and manage capture data?"
        ↓
Chapter 6
Working with Captured Packets
        ↓
"How do I actually analyze the traffic?"
```

This progression is important.

---

# 110. Final Chapter 4 Summary

Chapter 4 teaches the mechanics of getting network traffic into Wireshark.

The important workflow is:

```text
1. Find the correct interface
        ↓
2. Make sure you can see the traffic
        ↓
3. Configure capture options
        ↓
4. Decide whether to use a capture filter
        ↓
5. Start capture
        ↓
6. Observe traffic
        ↓
7. Stop or automatically stop capture
        ↓
8. Save/manage capture files
```

The deepest concept is:

> **Wireshark can only analyze traffic that it successfully captures.**

Therefore:

```text
Correct interface
        +
Correct capture location
        +
Correct capture settings
        +
Correct filter
        =
Useful capture
```

And remember the most important filtering distinction:

```text
Capture Filter
      ↓
Controls what gets captured

Display Filter
      ↓
Controls what gets displayed
      after packets have been captured
```

---

# Chapter 4 One-Line Memory

> **Chapter 4 = Learn how to correctly capture the right network traffic into Wireshark.**

```

This chapter is now best treated as your **capture foundation**. The official guide’s sequence is essentially **capture setup → interface/options → files → link-layer type → capture filters → running/stopping/restarting capture**. :contentReference[oaicite:1]{index=1}

**Your next major Wireshark milestone is Chapter 6 — “Working with Captured Packets.”** That's where we should slow down much more, because packet viewing, display filters, packet details, following streams, and actual investigation start becoming the main focus.
```
