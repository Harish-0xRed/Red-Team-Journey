````markdown
# Wireshark Chapter 3 — User Interface

> Source: Official Wireshark User's Guide
> Chapter: 3 — User Interface
> Purpose: Understand the Wireshark interface before learning packet analysis.
> Main idea: Learn where everything is and what each part of Wireshark does.

---

# 1. Introduction

Before analyzing packets, we need to understand the Wireshark interface.

Wireshark provides several areas for:

- Starting and stopping captures
- Opening capture files
- Filtering packets
- Navigating through packets
- Viewing protocol details
- Viewing raw packet bytes
- Viewing packet diagrams
- Checking statistics
- Managing Wireshark settings

The official chapter introduces the user interface before moving into:

```text
Chapter 3
User Interface
     ↓
Chapter 4
Capturing Live Network Data
     ↓
Chapter 5
File Input/Output
     ↓
Chapter 6
Working With Captured Packets
````

So Chapter 3 is the:

> "Learn how to operate Wireshark" chapter.

---

# 2. Starting Wireshark

Wireshark can be started from:

* A graphical application launcher
* A shell / terminal
* Command line

You can also provide command-line options when starting Wireshark.

Example:

```bash
wireshark
```

The exact appearance of Wireshark can differ between:

* Windows
* Linux
* macOS
* Different desktop environments
* Different GUI toolkit versions

The functionality is generally the same even when the visual appearance differs.

---

# 3. The Wireshark Main Window

The main Wireshark window contains several major components.

```text
┌──────────────────────────────────────────────┐
│ Menu                                         │
├──────────────────────────────────────────────┤
│ Main Toolbar                                 │
├──────────────────────────────────────────────┤
│ Filter Toolbar                               │
├──────────────────────────────────────────────┤
│                                              │
│ Packet List                                  │
│                                              │
├──────────────────────────────────────────────┤
│ Packet Details                               │
├──────────────────────────────────────────────┤
│ Packet Bytes                                 │
├──────────────────────────────────────────────┤
│ Packet Diagram                               │
├──────────────────────────────────────────────┤
│ Statusbar                                    │
└──────────────────────────────────────────────┘
```

The exact layout can be customized.

The important parts are:

1. Menu
2. Main Toolbar
3. Filter Toolbar
4. Packet List Pane
5. Packet Details Pane
6. Packet Bytes Pane
7. Packet Diagram Pane
8. Statusbar

---

# 4. The Big Three Packet Areas

For packet analysis, three panes are especially important:

```text
Packet List
     ↓
"What packets exist?"

Packet Details
     ↓
"What fields/protocols are inside this packet?"

Packet Bytes
     ↓
"What are the actual raw bytes?"
```

There is also:

```text
Packet Diagram
     ↓
"Show the packet structure visually."
```

This relationship is extremely important.

---

# 5. Packet List → Packet Details → Packet Bytes

Think about selecting one packet.

```text
Packet List
     │
     │ select packet
     ▼
Packet Details
     │
     │ select protocol field
     ▼
Packet Bytes
```

Example:

```text
Packet List

No.  Source       Destination      Protocol
10   10.0.0.5     10.0.0.1         TCP
11   10.0.0.1     10.0.0.5         TCP
12   10.0.0.5     10.0.0.1         HTTP
```

Select packet 12.

Wireshark then shows:

```text
Packet Details

Ethernet II
    ├── Destination
    ├── Source
    └── Type

Internet Protocol
    ├── Source
    ├── Destination
    └── TTL

Transmission Control Protocol
    ├── Source Port
    ├── Destination Port
    ├── Sequence Number
    └── Flags

HTTP
    ├── Request Method
    ├── Host
    └── URI
```

The Packet Bytes pane shows the actual bytes underneath these decoded fields.

---

# 6. Main Window Navigation

Wireshark supports keyboard navigation.

This is useful because packet analysis can involve moving through hundreds or thousands of packets.

---

## 6.1 Tab

```text
Tab
```

Moves between interface elements.

For example:

```text
Toolbar
   ↓
Packet List
   ↓
Packet Details
```

---

## 6.2 Shift + Tab

Moves backward through interface elements.

---

## 6.3 Down Arrow

```text
↓
```

Moves to the next packet or detail item.

---

## 6.4 Up Arrow

```text
↑
```

Moves to the previous packet or detail item.

---

## 6.5 Ctrl + Down / F8

```text
Ctrl + ↓
```

or:

```text
F8
```

Moves to the next packet even if the packet list does not currently have focus.

---

## 6.6 Ctrl + Up / F7

```text
Ctrl + ↑
```

or:

```text
F7
```

Moves to the previous packet even if the packet list does not currently have focus.

---

## 6.7 Ctrl + .

```text
Ctrl + .
```

Moves to the next packet in the current conversation.

For example:

```text
Packet 10
Packet 11
Packet 12
Packet 13
```

If packets 10 and 13 belong to the same conversation, this navigation can move between packets belonging to that conversation.

---

## 6.8 Ctrl + ,

```text
Ctrl + ,
```

Moves to the previous packet in the current conversation.

---

## 6.9 Selection History

Wireshark also maintains packet selection history.

```text
Alt + →
```

moves forward through packet selection history.

```text
Alt + ←
```

moves backward through packet selection history.

This is similar to:

```text
Browser Back
Browser Forward
```

but for packets you previously visited.

---

# 7. Navigating the Packet Details Tree

The Packet Details pane contains a tree.

Example:

```text
Ethernet II
    ▼
Internet Protocol Version 4
    ▼
Transmission Control Protocol
    ▼
Hypertext Transfer Protocol
```

You can expand and collapse these sections.

---

## Left Arrow

```text
←
```

Closes the selected tree item.

If it is already closed, it moves to the parent node.

---

## Right Arrow

```text
→
```

Opens the selected tree item.

---

## Shift + Right Arrow

```text
Shift + →
```

Opens the selected tree item and all its subtrees.

---

## Ctrl + Right Arrow

```text
Ctrl + →
```

Expands all tree items.

---

## Ctrl + Left Arrow

```text
Ctrl + ←
```

Collapses all tree items.

---

## Enter

```text
Enter
```

Toggles the selected tree item.

---

## Backspace

```text
Backspace
```

Moves to the parent node.

---

# 8. Keyboard Shortcuts

Wireshark contains many keyboard shortcuts.

You can view the available shortcuts through:

```text
View
  ↓
Internals
  ↓
Keyboard Shortcuts
```

Learning every shortcut is unnecessary.

Start with the ones you actually use.

Important examples:

```text
Ctrl + E
    Start/Stop capture

Ctrl + K
    Capture Options

Ctrl + O
    Open capture file

Ctrl + S
    Save

Ctrl + F
    Find packet

Ctrl + G
    Go to packet

Ctrl + ↓
    Next packet

Ctrl + ↑
    Previous packet
```

---

# 9. The Main Menu

Wireshark's main menu provides access to most of its functionality.

Main menus:

```text
File
Edit
View
Go
Capture
Analyze
Statistics
Telephony
Wireless
Tools
Help
```

Some menu items may be greyed out.

That normally means the action is currently unavailable.

Example:

```text
No capture loaded
      ↓
Save may be disabled
```

This is normal behavior.

---

# 10. File Menu

The File menu deals mainly with:

* Capture files
* Opening files
* Saving files
* Merging files
* Exporting data
* Printing
* Closing
* Quitting

---

## 10.1 Open

```text
File → Open
```

Shortcut:

```text
Ctrl + O
```

Opens an existing capture file.

Common examples:

```text
.pcap
.pcapng
```

---

# 11. Open Recent

Allows you to quickly reopen recently used capture files.

Useful when repeatedly working with the same captures.

---

# 12. Merge

```text
File → Merge
```

Combines another capture file with the currently loaded capture.

Concept:

```text
Capture A
   +
Capture B
   ↓
Merged capture
```

Detailed merging is covered later in Chapter 5.

---

# 13. Import from Hex Dump

```text
File → Import from Hex Dump
```

Allows Wireshark to import packet data represented as text/hexadecimal.

This is an advanced feature.

It is covered in Chapter 5.

---

# 14. Close

```text
Ctrl + W
```

Closes the current capture.

If the capture has not been saved, Wireshark may ask whether you want to save it.

---

# 15. Save

```text
Ctrl + S
```

Saves the current capture.

Important:

A live capture cannot be saved using this operation while the capture is still running.

You must stop the capture first.

---

# 16. Save As

```text
Ctrl + Shift + S
```

Allows you to save the current capture under a chosen filename.

Example:

```text
incident.pcapng
```

---

# 17. File Set

The File menu contains File Set operations.

You can:

```text
File Set → List Files
File Set → Next File
File Set → Previous File
```

These are useful when a capture consists of multiple files.

---

# 18. Export Specified Packets

Allows selected packets or ranges of packets to be exported to another file.

Concept:

```text
Large capture
      ↓
Select interesting packets
      ↓
Export
      ↓
Smaller capture
```

This is useful during investigations.

---

# 19. Export Packet Dissections

Exports decoded packet information rather than simply saving the original capture.

Possible formats include:

* Plain text
* CSV
* XML

This is useful when you want packet information in a format suitable for:

* Reports
* Documentation
* Scripts
* Further processing

---

# 20. Export Objects

Wireshark can export objects reconstructed from supported protocols.

Examples include objects from:

```text
HTTP
FTP-DATA
SMB
TFTP
DICOM
IMF
```

Concept:

```text
Captured packets
       ↓
Protocol reassembly
       ↓
Transferred object
       ↓
Export to file
```

This becomes particularly useful later in SOC investigations.

---

# 21. Print

```text
Ctrl + P
```

Allows packet information to be printed.

---

# 22. Quit

```text
Ctrl + Q
```

Exits Wireshark.

If there is an unsaved capture, Wireshark may ask whether it should be saved.

---

# 23. Edit Menu

The Edit menu provides tools for working with packets and Wireshark configuration.

Important functions include:

```text
Copy
Find Packet
Mark / Unmark
Ignore / Unignore
Time Reference
Time Shift
Packet Comment
TLS Secrets
ESP Secrets
Configuration Profiles
Preferences
```

---

# 24. Copy

Allows packet-related information to be copied to the clipboard.

Depending on what is selected, you can copy:

* Packet list information
* Packet details
* Packet properties

---

# 25. Find Packet

```text
Ctrl + F
```

Allows you to search for packets using various criteria.

Concept:

```text
Large capture
     ↓
Find
     ↓
Interesting packet
```

This is useful when manually navigating a large capture is inefficient.

---

# 26. Find Next / Find Previous

After creating a search:

```text
Ctrl + N
```

finds the next matching packet.

```text
Ctrl + B
```

finds the previous matching packet.

---

# 27. Marking Packets

You can mark packets.

```text
Ctrl + M
```

Concept:

```text
Packet 100
    ↓
MARKED
```

This is useful when building an investigation trail.

For example:

```text
Packet 100 → suspicious DNS
Packet 145 → suspicious connection
Packet 190 → HTTP request
```

You can later move between marked packets.

---

# 28. Mark All Displayed Packets

```text
Ctrl + Shift + M
```

Marks all currently displayed packets.

Important:

"Displayed" means packets currently visible according to the current display state/filter.

---

# 29. Unmark

Wireshark also provides options to:

```text
Unmark All Displayed
Next Mark
Previous Mark
```

This helps navigate selected investigation points.

---

# 30. Ignore Packets

Wireshark can mark packets as ignored.

```text
Ctrl + D
```

Concept:

```text
Packet
   ↓
Ignored
```

Ignored packets can be excluded from certain calculations and display behavior.

This is different from deleting a packet.

---

# 31. Time Reference

You can set a packet as a time reference.

```text
Ctrl + T
```

This lets you establish a useful time point while analyzing a capture.

Example:

```text
Attack begins
     ↓
Set Time Reference
     ↓
Analyze what happens afterward
```

You can also:

```text
Unset All Time References
Next Time Reference
Previous Time Reference
```

---

# 32. Time Shift

The Edit menu includes:

```text
Time Shift
```

This allows timestamps of packets to be adjusted.

This can be useful when working with captures whose timestamps need correction or alignment.

---

# 33. Packet Comments

You can add a comment to an individual packet.

Example:

```text
Packet 125

Comment:
Possible suspicious DNS request
```

This is useful for investigation notes.

Important:

The ability to store comments depends on the capture format.

For example:

```text
pcapng
    ↓
supports packet comments

pcap
    ↓
does not support packet comments
```

---

# 34. TLS and ESP Secrets

Wireshark can work with decryption secrets.

The Edit menu provides operations for:

```text
Inject TLS Secrets
Inject ESP Secrets
Discard All Secrets
```

These are advanced features.

Basic idea:

```text
Encrypted traffic
      +
Required decryption secrets
      ↓
Wireshark
      ↓
Possible protocol decryption
```

Do not worry about mastering this in Chapter 3.

You will encounter it later during packet analysis.

---

# 35. Configuration Profiles

Configuration Profiles allow Wireshark settings to be grouped.

Concept:

```text
Profile: SOC
    ↓
Certain filters/settings/colors

Profile: Pentest
    ↓
Different settings

Profile: Lab
    ↓
Different settings
```

This is useful when you use Wireshark for different tasks.

---

# 36. Preferences

```text
Edit → Preferences
```

Preferences control many aspects of Wireshark.

Examples include:

* Interface behavior
* Appearance
* Protocol settings
* Columns
* Name resolution
* Packet display
* Other application behavior

Preferences are covered more deeply in Chapter 11.

---

# 37. View Menu

The View menu controls how Wireshark displays information.

It can control:

```text
Toolbars
Packet panes
Statusbar
Time format
Name resolution
Zoom
Packet tree expansion
Packet colorization
```

---

# 38. Show / Hide Toolbars and Panes

You can show or hide:

```text
Main Toolbar
Filter Toolbar
Wireless Toolbar
Statusbar
Packet List
Packet Details
Packet Bytes
Packet Diagram
```

This is useful when you need more screen space.

Example:

```text
Small screen
    ↓
Hide unnecessary toolbar
    ↓
More packet data visible
```

---

# 39. Time Display Format

Wireshark provides multiple timestamp display formats.

Examples include:

```text
Time of Day
Date and Time of Day
Seconds Since Epoch
Seconds Since First Captured Packet
Seconds Since Previous Captured Packet
Seconds Since Previous Displayed Packet
```

---

# 40. Time of Day

Example:

```text
01:02:03.123456
```

Shows the packet's time of day.

---

# 41. Date and Time of Day

Example:

```text
1970-01-01 01:02:03.123456
```

Shows date + time.

---

# 42. Seconds Since Epoch

Example:

```text
1234567890.123456
```

This represents seconds since:

```text
1970-01-01 00:00:00
```

---

# 43. Seconds Since First Captured Packet

Example:

```text
123.123456
```

This answers:

> How much time has passed since the first captured packet?

Useful for analyzing event timing.

---

# 44. Seconds Since Previous Captured Packet

Shows the time difference from the previous captured packet.

Concept:

```text
Packet 1
   ↓
Packet 2
   ↓
Delta = time difference
```

---

# 45. Seconds Since Previous Displayed Packet

This differs from the previous captured packet.

It calculates the time since the previous packet that is currently displayed.

This distinction becomes useful when a display filter hides packets.

---

# 46. Timestamp Precision

Wireshark can display different levels of precision.

Examples:

```text
Seconds
Milliseconds
Microseconds
Nanoseconds
```

The actual available precision depends on the capture data.

---

# 47. Name Resolution

Wireshark can translate numeric addresses into names.

There are separate controls for:

```text
MAC layer
Network layer
Transport layer
```

Example concept:

```text
Numeric address
      ↓
Name resolution
      ↓
Human-readable name
```

Name resolution is discussed in more detail later.

---

# 48. Zoom

Wireshark allows the packet display font to be enlarged or reduced.

```text
Ctrl + +
```

Zoom in.

```text
Ctrl + -
```

Zoom out.

```text
Ctrl + 0
```

Return to normal size.

---

# 49. Expand and Collapse Packet Trees

View menu provides:

```text
Expand Subtrees
Collapse Subtrees
Expand All
Collapse All
```

Useful when analyzing complex packets.

---

# 50. Packet Colorization

Wireshark can color packet rows.

Colorization makes packet types easier to distinguish visually.

Example:

```text
TCP traffic
DNS traffic
HTTP traffic
Errors
Warnings
```

may appear differently depending on configured coloring rules.

Important:

Colorization can slow down display of new packets while capturing or loading capture files.

---

# 51. Conversation Colorization

Wireshark can temporarily color packets based on the addresses of the selected conversation.

This can make it easier to distinguish traffic belonging to different conversations.

There are temporary color filters:

```text
Color 1
Color 2
...
Color 10
```

You can also reset temporary coloring.

---

# 52. Go Menu

The Go menu is about navigation.

It lets you move around the capture efficiently.

Important actions:

```text
Go to Packet
Go to Linked Packet
Next Packet
Previous Packet
First Packet
Last Packet
Next Packet in Conversation
Previous Packet in Conversation
First Packet in Conversation
Last Packet in Conversation
Packet History
```

---

# 53. Go to Packet

Shortcut:

```text
Ctrl + G
```

Enter a packet number.

Example:

```text
Capture:
1
2
3
...
50000
```

You want:

```text
Packet 35000
```

Use:

```text
Ctrl + G
```

and enter:

```text
35000
```

---

# 54. Go to Linked Packet

Sometimes a protocol field is associated with another packet.

For example:

```text
Request
   ↓
Response
```

Wireshark may provide a link between them.

Selecting:

```text
Go to Linked Packet
```

jumps to the corresponding packet.

---

# 55. First / Last Packet

```text
Ctrl + Home
```

goes to the first packet.

```text
Ctrl + End
```

goes to the last packet.

---

# 56. Conversation Navigation

Wireshark can navigate within the current conversation.

```text
Next packet in conversation
Previous packet in conversation
First packet in conversation
Last packet in conversation
```

This becomes extremely useful later when analyzing:

```text
TCP sessions
UDP conversations
IP traffic
```

---

# 57. Packet History

Wireshark maintains packet selection history.

This behaves similarly to browser navigation.

Example:

```text
Packet 10
   ↓
Packet 50
   ↓
Packet 100
   ↓
Packet 500
```

You can move backward through the history.

---

# 58. Capture Menu

The Capture menu controls live packet capture.

Main options include:

```text
Options
Start
Stop
Restart
Capture Filters
Refresh Interfaces
```

---

# 59. Capture Options

```text
Ctrl + K
```

opens Capture Options.

This is where you configure:

* Capture interfaces
* Promiscuous mode
* Capture filters
* Buffer
* Snaplen
* Monitor mode
* Output files
* Multiple-file capture
* Ring buffers
* Stop conditions

These were covered deeply in Chapter 4.

---

# 60. Start Capture

```text
Ctrl + E
```

starts capturing.

Wireshark uses the current/default capture settings.

---

# 61. Stop Capture

```text
Ctrl + E
```

stops a running capture.

Interesting detail:

The same shortcut is used for Start and Stop depending on the current state.

---

# 62. Restart Capture

```text
Ctrl + R
```

Stops the current capture and starts it again using the same options.

Concept:

```text
Current capture
      ↓
Restart
      ↓
Old capture ends
      ↓
New capture starts
```

---

# 63. Capture Filters

The Capture menu provides access to capture filter management.

You can:

* Create filters
* Edit filters
* Name filters
* Save filters
* Reuse filters

Remember:

```text
Capture Filter
    ↓
Controls packets captured
```

This is different from a display filter.

---

# 64. Refresh Interfaces

Shortcut:

```text
F5
```

Refreshes the interface list.

Useful if:

* A new network interface appeared
* A VPN interface was created
* An interface disappeared
* Network configuration changed

---

# 65. Analyze Menu

The Analyze menu contains packet-analysis related actions.

Important areas include:

```text
Display Filters
Display Filter Macros
Display Filter Expression
Apply as Column
Apply as Filter
Prepare as Filter
Conversation Filter
Protocol dissection
Decode As
Follow streams
```

---

# 66. Display Filters

Display filters control what is shown from packets that have already been captured.

Example:

```text
http
```

or:

```text
ip.addr == 192.168.1.10
```

Concept:

```text
Captured packets
       ↓
Display Filter
       ↓
Only matching packets displayed
```

The packets are not deleted.

---

# 67. Display Filter Macros

Wireshark allows reusable display filter macros.

Concept:

```text
Complex filter
      ↓
Save as macro
      ↓
Reuse later
```

Useful when a filter becomes complicated.

---

# 68. Display Filter Expression

Wireshark provides a dialog that helps construct display filters.

It can show:

* Available fields
* Relationships/operators
* Possible values
* Field descriptions

This is especially useful while learning display filters.

---

# 69. Apply as Column

A protocol field selected in Packet Details can be added as a Packet List column.

Example:

```text
Packet Details
      ↓
Select HTTP Host
      ↓
Apply as Column
      ↓
New Packet List column
```

This is very useful during investigations.

---

# 70. Apply as Filter

A selected protocol field can be used to create/apply a display filter.

Concept:

```text
Select interesting field
        ↓
Apply as Filter
        ↓
Wireshark creates filter
        ↓
Only matching packets displayed
```

This is one of the easiest ways to learn display-filter syntax.

---

# 71. Prepare as Filter

Similar to Apply as Filter, but prepares the filter without immediately applying it.

Useful when you want to modify the expression first.

---

# 72. Conversation Filter

Allows filtering based on the current conversation.

For example:

```text
Source
Destination
Protocol
Ports
```

This is useful when you find an interesting session and want to isolate it.

---

# 73. Statistics Menu

The Statistics menu opens windows containing statistical information about the capture.

Important categories include:

```text
Capture File Properties
Resolved Addresses
Protocol Hierarchy
Conversations
Endpoints
Packet Lengths
I/O Graphs
Plots
Service Response Time
Distribution
```

And many protocol-specific statistics.

---

# 74. Capture File Properties

Provides information about the loaded capture file.

Useful for understanding:

```text
Capture metadata
File properties
Packet counts
Timing
Other capture information
```

---

# 75. Protocol Hierarchy

Shows protocols in a hierarchical structure.

Concept:

```text
Ethernet
 └── IP
      └── TCP
           └── HTTP
```

It can also provide statistics for the protocols found in the capture.

This becomes useful when asking:

> "What protocols are actually present in this capture?"

---

# 76. Conversations

A conversation represents traffic between endpoints.

Concept:

```text
10.0.0.5
    ↕
10.0.0.10
```

Wireshark can display conversation statistics.

This is very useful for network investigation.

---

# 77. Endpoints

Endpoints represent network addresses involved in traffic.

Concept:

```text
10.0.0.5
10.0.0.10
10.0.0.20
```

You can use endpoint information to understand:

> Which systems are participating in the capture?

---

# 78. Packet Lengths

Provides statistics about packet sizes.

This can help identify:

* Typical packet sizes
* Large packets
* Small packets
* Packet-size distributions

---

# 79. I/O Graphs

I/O graphs allow you to visualize traffic over time.

Concept:

```text
Traffic
  │
  │       /\
  │      /  \
  │  ___/    \____
  │
  └──────────────── Time
```

Useful for finding:

* Traffic spikes
* Quiet periods
* Bursts
* Timing patterns

---

# 80. Service Response Time

Measures the time between a request and corresponding response for supported protocols.

Concept:

```text
Request
   │
   │
   │  response time
   │
   ▼
Response
```

This can help identify slow services.

---

# 81. Distribution Statistics

Can display distributions and entropy for selected named fields.

This is more advanced.

For now:

> Understand that Wireshark can statistically analyze values appearing in captured traffic.

---

# 82. Telephony Menu

The Telephony menu provides specialized analysis for telephony-related protocols and traffic.

Examples include:

```text
VoIP Calls
RTP
SIP
SCTP
LTE
GSM
ISUP
SMPP
```

These features are useful when analyzing:

* Voice traffic
* Signaling
* Mobile networks
* Real-time media

For your current networking/SOC learning:

> Recognize this menu, but don't prioritize it yet.

---

# 83. Wireless Menu

The Wireless menu focuses on:

* Bluetooth
* IEEE 802.11 / WLAN

Examples include:

```text
Bluetooth ATT Server Attributes
Bluetooth Devices
Bluetooth HCI Summary
WLAN Traffic
```

This becomes more important when you begin deeper wireless analysis.

---

# 84. Tools Menu

The Tools menu contains additional Wireshark utilities.

One example documented in the guide is creating firewall ACL rules.

This menu is more specialized than the core packet-analysis workflow.

For now:

> Recognize it and return to it when a practical task requires it.

---

# 85. Help Menu

The Help menu provides access to:

```text
User's Guide
Manual Pages
Website
FAQs
Downloads
Wiki
Sample Captures
Release Notes
About Wireshark
Check for Updates
```

This is particularly useful for learning.

---

# 86. User's Guide

Shortcut:

```text
F1
```

opens the User's Guide.

This is the documentation you are currently using as your main Wireshark textbook.

---

# 87. Wiki

The Wireshark Wiki contains additional information and resources.

It can be useful for:

* Protocol information
* Examples
* Special topics
* Sample captures
* Community knowledge

---

# 88. Sample Captures

Wireshark provides sample capture resources.

These are extremely useful for learning because you can practice analysis without needing to generate every type of traffic yourself.

Mental model:

```text
Sample PCAP
     ↓
Open in Wireshark
     ↓
Practice analysis
```

---

# 89. About Wireshark

The About dialog provides information about:

* Wireshark build
* Loaded plugins
* Folders
* Build information
* Other application details

Useful when troubleshooting installations or plugin issues.

---

# 90. Main Toolbar

The Main Toolbar provides quick access to frequently used actions.

It is essentially a shortcut to common menu operations.

Important buttons include:

```text
Start
Stop
Restart
Options
Open
Save As
Close
```

---

# 91. Start Button

Starts a packet capture.

Equivalent to:

```text
Capture → Start
```

---

# 92. Stop Button

Stops the currently running capture.

Equivalent to:

```text
Capture → Stop
```

---

# 93. Restart Button

Restarts the current capture.

Equivalent to:

```text
Capture → Restart
```

---

# 94. Options Button

Opens:

```text
Capture Options
```

This is where capture configuration is performed.

---

# 95. Open Button

Opens a capture file.

Equivalent to:

```text
File → Open
```

---

# 96. Save As Button

Saves the current capture under a chosen filename.

---

# 97. Close Button

Closes the current capture.

If necessary, Wireshark asks whether the capture should be saved.

---

# 98. Toolbar Buttons Can Be Disabled

Some buttons appear greyed out.

This usually means the corresponding action is not currently possible.

Example:

```text
No capture running
      ↓
Stop button disabled
```

This is normal.

---

# 99. Filter Toolbar

The Filter Toolbar is one of the most important areas for future packet analysis.

It allows you to:

* Enter display filters
* Edit display filters
* Apply filters
* Clear filters
* Use recent filters
* Manage saved filters
* Create filter buttons

---

# 100. Filter Input

This is where you type a display filter.

Example:

```text
ip.addr == 192.168.1.10
```

Wireshark performs syntax checking while you type.

The input area changes color depending on validity.

Concept:

```text
Invalid/incomplete
      ↓
Red

Valid
      ↓
Green
```

---

# 101. Apply Button

After entering a filter, press:

```text
Apply
```

or:

```text
Enter / Return
```

to apply it.

Concept:

```text
Type filter
    ↓
Syntax check
    ↓
Apply
    ↓
Packet list changes
```

---

# 102. Clear Button

Clears the current display filter.

Concept:

```text
Filtered view
      ↓
Clear
      ↓
All captured packets displayed again
```

---

# 103. Recent Filters

Wireshark remembers recently applied display filters.

This allows you to quickly reuse filters.

---

# 104. Filter Bookmarks

Bookmarks can be used to manage saved filters.

Concept:

```text
Frequently used filter
        ↓
Save
        ↓
Bookmark
        ↓
Reuse later
```

---

# 105. Filter Buttons

Filter buttons provide one-click shortcuts.

Example:

```text
[ HTTP ]
[ DNS ]
[ TCP ]
[ Suspicious ]
```

Clicking one immediately applies its associated display filter.

This can be very useful for repetitive SOC workflows.

---

# 106. Grouping Filter Buttons

Filter buttons can be grouped using:

```text
//
```

Example:

```text
Not Squirrels // Rabbits
Not Squirrels // Capybaras
```

Wireshark can group these under:

```text
Not Squirrels
```

This is a convenience feature.

---

# 107. Packet List Pane

The Packet List pane displays packets in the current capture.

Important:

> One row represents one packet in the capture.

Example:

```text
No.   Time      Source       Destination    Protocol
1     0.000     10.0.0.5     10.0.0.1       TCP
2     0.001     10.0.0.1     10.0.0.5       TCP
3     0.010     10.0.0.5     10.0.0.1       HTTP
```

---

# 108. Selecting a Packet

When you select a packet:

```text
Packet List
     ↓
Packet Details updates
     ↓
Packet Bytes updates
     ↓
Packet Diagram updates
```

This is one of the most important UI relationships.

---

# 109. Packet List Columns

Default columns include:

```text
No.
Time
Delta
Source
Destination
Protocol
```

Other columns can be configured.

---

# 110. No. Column

The packet number in the capture file.

Example:

```text
1
2
3
4
...
```

Important:

> Packet numbers do not change simply because a display filter is applied.

Example:

```text
Original:

1
2
3
4
5
```

Apply a display filter that hides packets 2 and 4:

```text
Displayed:

1
3
5
```

The packet numbers remain:

```text
1
3
5
```

They are not renumbered to:

```text
1
2
3
```

---

# 111. Time Column

Shows the timestamp associated with the packet.

The format can be changed through:

```text
View
  ↓
Time Display Format
```

---

# 112. Delta Column

Shows time difference relative to the previous displayed packet.

This is useful when analyzing packet timing.

Example:

```text
Packet 1   0.000000
Packet 2   0.001000
Packet 3   0.050000
```

You can identify timing gaps.

---

# 113. Source Column

Shows where the packet is coming from.

Depending on the protocol, this may represent:

* MAC address
* IP address
* Port/address information

---

# 114. Destination Column

Shows where the packet is going.

---

# 115. Protocol Column

Shows the highest-level protocol Wireshark has identified for that packet.

Example:

```text
TCP
HTTP
DNS
TLS
SSH
```

Important:

Wireshark's dissectors process protocol layers.

Higher-level protocol information can overwrite lower-level information in the packet list columns.

---

# 116. Example of Protocol Layering

Suppose:

```text
Ethernet
   ↓
IP
   ↓
TCP
   ↓
HTTP
```

During dissection:

```text
Ethernet
   ↓
writes Ethernet information

IP
   ↓
writes IP information

TCP
   ↓
writes TCP information

HTTP
   ↓
writes HTTP information
```

Therefore, the packet list usually shows information from the highest protocol level available.

This is why you may see:

```text
HTTP
```

rather than:

```text
Ethernet
```

in the Protocol column.

---

# 117. Packet Details Pane

The Packet Details pane shows the selected packet in much greater detail.

It presents:

```text
Protocols
    ↓
Fields
    ↓
Values
```

in a tree structure.

Example:

```text
Frame
Ethernet II
Internet Protocol Version 4
Transmission Control Protocol
Hypertext Transfer Protocol
```

Each section can be expanded.

---

# 118. Protocol Tree

Example:

```text
Ethernet II
    ├── Destination
    ├── Source
    └── Type

IPv4
    ├── Version
    ├── Header Length
    ├── Total Length
    ├── TTL
    └── Protocol

TCP
    ├── Source Port
    ├── Destination Port
    ├── Sequence Number
    ├── Acknowledgment Number
    └── Flags
```

This is where most packet analysis will eventually happen.

---

# 119. Generated Fields

Some information shown by Wireshark is not literally present as bytes in the capture.

Wireshark can generate additional information.

Generated fields are displayed in:

```text
[ ... ]
```

Examples include:

* Response times
* TCP analysis
* IP geolocation information
* Checksum validation

Important concept:

```text
Captured bytes
      +
Wireshark's analysis
      ↓
Displayed information
```

Not every displayed value is a raw field copied directly from the packet.

---

# 120. Links in Packet Details

Wireshark can detect relationships between packets.

For example:

```text
Request
   ↓
Response
```

A linked field may appear as an underlined blue link.

Double-clicking it can take you to the related packet.

This is very useful during investigations.

---

# 121. Packet Bytes Pane

The Packet Bytes pane shows the raw packet data in hexadecimal form.

Typical layout:

```text
Offset     Hexadecimal bytes                  ASCII
00000000   00 11 22 33 44 55 ...             .."3DU...
00000010   45 00 00 3c ...                    E..<...
```

Each line normally contains:

```text
Offset
16 hexadecimal bytes
16 ASCII representations
```

---

# 122. Offset

The offset indicates the position of a byte within the packet.

Example:

```text
00000000
00000010
00000020
```

These are hexadecimal offsets.

For example:

```text
0x10 = 16 decimal
```

---

# 123. Hexadecimal Bytes

The middle section displays raw bytes.

Example:

```text
45 00 00 3c
```

Each pair represents one byte.

Example:

```text
45
00
00
3c
```

means four bytes.

---

# 124. ASCII Representation

The right side displays printable bytes as ASCII characters.

Non-printable bytes are shown as:

```text
.
```

Example:

```text
48 65 6c 6c 6f
```

corresponds to:

```text
Hello
```

This becomes useful when examining:

* HTTP data
* DNS names
* Text protocols
* Payloads
* Application data

---

# 125. Selecting a Field Highlights Bytes

When you select or hover over a protocol field in Packet Details, Wireshark can highlight the corresponding bytes in Packet Bytes.

Concept:

```text
Packet Details
      ↓
Select field
      ↓
Corresponding raw bytes highlighted
```

This creates a direct connection between:

```text
Protocol meaning
        ↕
Raw bytes
```

This is an extremely important skill for deeper packet analysis.

---

# 126. Multiple Data Sources in Packet Bytes

Sometimes Wireshark has more than one representation of packet data.

For example:

```text
Reassembled data
Decrypted data
Other protocol data sources
```

These can appear as additional tabs in the Packet Bytes pane.

---

# 127. Packet Reassembly

Sometimes application data is split across multiple packets.

Example:

```text
TCP Segment 1
      +
TCP Segment 2
      +
TCP Segment 3
      ↓
Reassembled application data
```

Wireshark can reassemble these pieces.

The Packet Bytes pane may then provide additional data views.

---

# 128. Packet Diagram Pane

The Packet Diagram pane presents the selected packet visually.

It resembles diagrams commonly used in:

* Textbooks
* Protocol documentation
* IETF RFCs

Concept:

```text
Ethernet
┌───────────────────────────┐
│ Ethernet Header           │
├───────────────────────────┤
│ IPv4 Header               │
├───────────────────────────┤
│ TCP Header                │
├───────────────────────────┤
│ Application Data          │
└───────────────────────────┘
```

The exact diagram depends on the packet.

---

# 129. Why Packet Diagram Is Useful

Packet Details gives you:

```text
Tree structure
```

Packet Bytes gives:

```text
Raw bytes
```

Packet Diagram gives:

```text
Visual packet structure
```

So:

```text
Packet Details
    ↓
Detailed fields

Packet Bytes
    ↓
Raw bytes

Packet Diagram
    ↓
Visual structure
```

---

# 130. Statusbar

The Statusbar displays information about Wireshark's current state.

Generally:

```text
Left
    ↓
Context-related information

Middle
    ↓
Capture file information

Right
    ↓
Current configuration profile
```

The areas can be resized.

---

# 131. Expert Information Indicator

The statusbar can show a colored expert-information indicator.

It represents the highest expert information level found in the currently loaded capture.

Hovering provides information.

Clicking it opens the Expert Information dialog.

This is important later when investigating:

* Warnings
* Errors
* Suspicious protocol behavior
* Protocol anomalies

---

# 132. Capture File Comment

The statusbar can provide access to capture-file comments.

This allows you to add comments to the capture file.

Useful for:

```text
Incident investigation
Lab notes
Case information
Analysis context
```

---

# 133. Statusbar Packet Counts

The middle area can display packet counts.

Important values include:

```text
Packets
Displayed
Marked
Dropped
Ignored
```

---

# 134. Packets

Represents:

> Number of captured packets.

Example:

```text
Packets: 5000
```

---

# 135. Displayed

Represents:

> Number of packets currently displayed.

Example:

```text
Packets:   5000
Displayed: 250
```

This could happen after applying a display filter.

---

# 136. Marked

Represents:

> Number of marked packets.

This appears when packets have been marked.

---

# 137. Dropped

Represents:

> Packets that Wireshark was unable to capture.

Example:

```text
Captured: 10000
Dropped:  200
```

This is important because dropped packets mean the capture may not contain everything that actually occurred.

---

# 138. Ignored

Represents:

> Packets that have been marked as ignored.

---

# 139. Configuration Profile

The right side of the statusbar shows the currently selected configuration profile.

Example:

```text
Default
SOC-Lab
Pentest
Forensics
```

The exact profiles depend on what you create.

---

# 140. The Complete Wireshark UI Mental Model

Think of Wireshark as a layered workspace:

```text
                 WIRESHARK
                     │
       ┌─────────────┴─────────────┐
       │                           │
    Controls                    Analysis
       │                           │
       ▼                           ▼
    Menus                      Packet List
    Toolbars                       │
    Capture                        ▼
    Filters                    Packet Details
                                    │
                                    ▼
                               Packet Bytes
                                    │
                                    ▼
                               Raw Data
```

---

# 141. The Most Important Workflow

When you open a capture:

```text
1. Look at Packet List
        ↓
2. Find an interesting packet
        ↓
3. Select it
        ↓
4. Read Packet Details
        ↓
5. Expand protocol trees
        ↓
6. Select interesting fields
        ↓
7. Observe corresponding bytes
        ↓
8. Use filters to narrow traffic
        ↓
9. Navigate related packets
```

This is the workflow you will repeatedly use later.

---

# 142. Example — Investigating an HTTP Packet

Suppose you have:

```text
Packet List
```

and find:

```text
HTTP
```

Select it.

Then:

```text
Packet Details
       ↓
Ethernet
       ↓
IP
       ↓
TCP
       ↓
HTTP
```

Expand HTTP.

You may see fields such as:

```text
Request Method
Host
URI
User-Agent
```

Select one field.

Then:

```text
Packet Bytes
```

shows the corresponding raw bytes.

The complete relationship becomes:

```text
Packet List
    ↓
"This packet is HTTP"
    ↓
Packet Details
    ↓
"This is the Host field"
    ↓
Packet Bytes
    ↓
"These are the actual bytes representing it"
```

---

# 143. Example — Investigating a TCP Packet

Packet List:

```text
TCP
```

Packet Details:

```text
Transmission Control Protocol
    ├── Source Port
    ├── Destination Port
    ├── Sequence Number
    ├── Acknowledgment Number
    ├── Flags
    └── Window
```

Packet Bytes:

```text
Raw TCP header bytes
```

This allows you to connect:

```text
TCP concept
     ↕
Wireshark field
     ↕
Raw bytes
```

This is how Wireshark becomes a networking-learning tool rather than simply a packet viewer.

---

# 144. Display Filter Mental Model

The filter toolbar should become part of your normal workflow.

Example:

```text
Capture
   ↓
10,000 packets
   ↓
Display Filter
   ↓
200 packets
   ↓
Investigate
```

The filter does not mean:

```text
Delete 9,800 packets
```

It means:

```text
Only show matching packets.
```

---

# 145. Capture Filter vs Display Filter

This distinction must be permanently remembered.

## Capture Filter

Used before/during capture.

```text
Network
   ↓
Capture Filter
   ↓
Captured packets
```

It determines what enters the capture.

---

## Display Filter

Used after packets are captured.

```text
Captured packets
       ↓
Display Filter
       ↓
Packets currently displayed
```

It determines what you see.

---

# 146. Navigation vs Filtering

These are different operations.

## Navigation

```text
Go to packet 500
```

means:

> Take me to packet 500.

## Filtering

```text
tcp
```

means:

> Show packets matching TCP.

You will use both constantly.

---

# 147. UI Components — Quick Table

| Component      | Main Purpose                            |
| -------------- | --------------------------------------- |
| Menu           | Access Wireshark functions              |
| Main Toolbar   | Quick access to common actions          |
| Filter Toolbar | Enter/apply display filters             |
| Packet List    | Summary of packets                      |
| Packet Details | Detailed protocol fields                |
| Packet Bytes   | Raw packet bytes                        |
| Packet Diagram | Visual packet structure                 |
| Statusbar      | Current capture/application information |

---

# 148. Important Menus — Quick Table

| Menu       | Main Purpose                                  |
| ---------- | --------------------------------------------- |
| File       | Open/save/export/close captures               |
| Edit       | Find, mark, comments, profiles, preferences   |
| View       | Control display/layout/time/colorization      |
| Go         | Navigate through packets                      |
| Capture    | Start/stop/configure capture                  |
| Analyze    | Filters, protocol analysis, stream operations |
| Statistics | Traffic statistics                            |
| Telephony  | Telephony/VoIP analysis                       |
| Wireless   | Bluetooth/WLAN analysis                       |
| Tools      | Additional utilities                          |
| Help       | Documentation/resources                       |

---

# 149. Packet Panes — Quick Table

| Pane           | Question It Answers                         |
| -------------- | ------------------------------------------- |
| Packet List    | "Which packets do I have?"                  |
| Packet Details | "What protocols/fields are inside?"         |
| Packet Bytes   | "What are the raw bytes?"                   |
| Packet Diagram | "What does the packet structure look like?" |
| Statusbar      | "What is the current state/count?"          |

---

# 150. Chapter 3 — What You Must Remember

## Level 1 — MUST KNOW

### 1. Packet List

> One row represents one packet.

### 2. Packet Details

> Shows the selected packet's protocols and fields.

### 3. Packet Bytes

> Shows the raw packet data in hexadecimal and ASCII.

### 4. Packet Diagram

> Shows a visual representation of the packet structure.

### 5. Filter Toolbar

> Used to apply display filters.

### 6. Capture Menu

> Used to control packet capture.

### 7. Analyze Menu

> Used for packet analysis and display-filter operations.

### 8. Statistics Menu

> Provides statistical views of the capture.

### 9. Statusbar

> Shows packet counts and current Wireshark/capture information.

---

# 151. Level 2 — SHOULD UNDERSTAND

Understand:

```text
Packet List
     ↓
Packet Details
     ↓
Packet Bytes
```

Understand:

```text
Capture Filter
        ≠
Display Filter
```

Understand:

```text
Captured
    ≠
Displayed
```

Understand:

```text
Packet number
    ≠
Displayed packet position
```

Understand:

```text
Raw bytes
    ↕
Decoded protocol fields
```

---

# 152. Level 3 — Recognize for Later

You do not need to master these now:

```text
Telephony menu
Wireless statistics
Configuration profiles
TLS secret injection
ESP secret injection
Filter macros
Packet colorization rules
Distribution statistics
Specialized tools
```

Know that they exist.

Learn them when the relevant topic arrives.

---

# 153. The Most Important Mental Model

Wireshark is not simply:

```text
"Packet viewer"
```

It is a complete packet-analysis environment.

The basic flow is:

```text
             CAPTURE
                ↓
        ┌───────────────┐
        │ Packet List   │
        └───────┬───────┘
                ↓
        Select a packet
                ↓
        ┌───────────────┐
        │Packet Details │
        └───────┬───────┘
                ↓
        Select a field
                ↓
        ┌───────────────┐
        │ Packet Bytes  │
        └───────────────┘
                ↓
        Understand raw data
```

At the same time:

```text
Filter Toolbar
      ↓
Narrow the packet set
```

and:

```text
Go Menu
      ↓
Navigate through packets
```

and:

```text
Statistics
      ↓
Understand traffic patterns
```

---

# 154. Chapter 3 → Chapter 4

Chapter 3 taught:

> How to operate Wireshark.

Chapter 4 teaches:

> How to capture network traffic.

Relationship:

```text
Chapter 3
Learn the interface
       ↓
Chapter 4
Capture traffic
       ↓
Chapter 5
Manage capture files
       ↓
Chapter 6
Analyze captured packets
```

---

# 155. Chapter 3 → Chapter 6

The eventual packet-analysis workflow becomes:

```text
                    Wireshark
                        │
                        ▼
                 Capture / Open PCAP
                        │
                        ▼
                  Packet List
                        │
                        ▼
                 Select Packet
                        │
              ┌─────────┴─────────┐
              ▼                   ▼
       Packet Details        Packet Bytes
              │
              ▼
       Protocol Analysis
              │
              ▼
       Display Filters
              │
              ▼
       Conversation Analysis
              │
              ▼
       Statistics / Streams
              │
              ▼
          Investigation
```

---

# 156. Practical Skills From Chapter 3

Before moving deeper into packet analysis, you should be comfortable doing these:

```text
[ ] Open Wireshark

[ ] Open a PCAP/PCAPNG file

[ ] Identify the Packet List

[ ] Select a packet

[ ] Expand Packet Details

[ ] Collapse Packet Details

[ ] Identify Ethernet/IP/TCP/UDP/application layers

[ ] Find raw bytes in Packet Bytes

[ ] Understand hexadecimal offsets

[ ] Use the display filter toolbar

[ ] Clear a display filter

[ ] Navigate to a packet number

[ ] Move between packets

[ ] Move between packets in a conversation

[ ] Understand packet counts in the statusbar

[ ] Start/stop a capture

[ ] Open Capture Options
```

---

# 157. Hands-On Exercise

Use any small `.pcap` or `.pcapng` capture.

## Exercise 1 — Packet List

Open the capture.

Identify:

```text
No.
Time
Source
Destination
Protocol
```

---

## Exercise 2 — Packet Details

Select one TCP packet.

Expand:

```text
Ethernet
IP
TCP
```

Identify:

```text
Source
Destination
Source Port
Destination Port
Flags
```

---

## Exercise 3 — Packet Bytes

Select:

```text
Source IP
```

in Packet Details.

Look at Packet Bytes.

Observe the bytes highlighted by Wireshark.

---

## Exercise 4 — Navigation

Use:

```text
Ctrl + G
```

Jump to a packet number.

Then use:

```text
Ctrl + ↓
Ctrl + ↑
```

to move around.

---

## Exercise 5 — Conversation Navigation

Select a TCP packet.

Use:

```text
Ctrl + .
Ctrl + ,
```

Observe how Wireshark moves through packets belonging to the conversation.

---

## Exercise 6 — Display Filter

Try:

```text
tcp
```

Then:

```text
udp
```

Then:

```text
dns
```

Then clear the filter.

Observe:

```text
Packets
Displayed
```

in the statusbar.

---

# 158. Final Chapter 3 Summary

Chapter 3 teaches you how Wireshark is organized.

The most important mental model is:

```text
             WIRESHARK
                 │
       ┌─────────┴─────────┐
       │                   │
    Controls            Analysis
       │                   │
       ▼                   ▼
    Menus             Packet List
    Toolbars               │
    Capture                ▼
    Filters           Packet Details
                           │
                           ▼
                      Packet Bytes
                           │
                           ▼
                       Raw Bytes
```

The four most important areas are:

```text
1. Packet List
      ↓
   What packets exist?

2. Packet Details
      ↓
   What protocols and fields are inside?

3. Packet Bytes
      ↓
   What are the actual bytes?

4. Filter Toolbar
      ↓
   Which captured packets do I want to see?
```

---

# 159. One-Line Memory

> **Chapter 3 = Learn how to navigate Wireshark, control its interface, find packets, filter what you see, and connect decoded protocol fields to raw packet bytes.**

---

# 160. Final Learning Position

After completing Chapter 3:

```text
Chapter 3
User Interface
        ✓
        ↓
Chapter 4
Capturing Live Network Data
        ✓
        ↓
Chapter 5
File Input/Output
        ✓
        ↓
Chapter 6
Working With Captured Packets
        ↓
        THIS IS WHERE
        DEEP PACKET ANALYSIS
        REALLY STARTS
```

Chapter 3 is therefore not about becoming good at packet analysis yet.

It gives you the controls and mental map required to perform packet analysis efficiently.

````

### 🧠 The key thing I want you to take from Chapter 3

Don't try to memorize every menu item. The **real foundation** is this:

```text
                 Wireshark
                     │
                     ▼
              ┌─────────────┐
              │ Packet List │
              └──────┬──────┘
                     │
              Select packet
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
 Packet Details            Packet Bytes
 "What does it mean?"      "What are the bytes?"
          │
          ▼
     Apply Filter
          │
          ▼
     Investigate
````
