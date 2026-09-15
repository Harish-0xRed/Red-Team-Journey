# Wireshark Chapter 6 — Working With Captured Packets

## Chapter Overview

Chapter 6 is about what to do AFTER packets have been captured.

The previous Wireshark chapters introduced the interface, capturing packets, and working with capture files.

Chapter 6 moves into actual packet handling:

- Viewing packets
- Understanding Packet List, Packet Details, and Packet Bytes
- Using context/pop-up menus
- Filtering packets
- Building display filters
- Finding packets
- Navigating through packets
- Marking and ignoring packets
- Working with packet timestamps
- Creating time references
- Shifting timestamps

The main idea of Chapter 6 is:

    Capture packets
          ↓
    View packets
          ↓
    Understand packet structure
          ↓
    Filter interesting packets
          ↓
    Find and navigate packets
          ↓
    Mark important packets
          ↓
    Analyze timing
          ↓
    Prepare for deeper packet analysis


---

# 6.1 — Viewing Packets You Have Captured

After capturing packets or opening a capture file, packets appear in the Packet List pane.

Selecting a packet causes Wireshark to show information about that packet in:

1. Packet List
2. Packet Details / protocol tree
3. Packet Bytes

The basic workflow is:

    Packet List
         ↓
    Select packet
         ↓
    Packet Details
         ↓
    Select a field
         ↓
    Corresponding bytes highlighted


## Packet List

The Packet List gives a summary of captured packets.

Typical columns include:

- Packet number
- Time
- Source
- Destination
- Protocol
- Length
- Info

The Packet List is useful for getting a quick overview of traffic.


## Packet Details

The Packet Details pane shows the selected packet as a protocol tree.

For example:

    Frame
      └── Ethernet
            └── IPv4
                  └── TCP
                        └── Application data

Each protocol can be expanded to inspect its fields.

Selecting a field in the protocol tree highlights the corresponding bytes in Packet Bytes.


## Packet Bytes

The Packet Bytes pane shows the raw bytes of the selected packet.

It normally contains:

- Offset
- Hexadecimal bytes
- Text/ASCII representation

Selecting a field in Packet Details causes Wireshark to highlight the bytes belonging to that field.


## Separate Packet Window

A packet can also be opened in a separate window.

This is useful when comparing:

- Two packets
- Multiple packets
- Packets from different capture files

A packet can be opened by double-clicking it or using:

    View → Show Packet in New Window

The separate packet window also provides a way to search within that packet.


## Core Mental Model

    Packet List
        ↓
    "Which packet?"

    Packet Details
        ↓
    "What protocols and fields are inside?"

    Packet Bytes
        ↓
    "What are the actual bytes?"

This is one of the most important mental models in Wireshark.


---

# 6.2 — Pop-up Menus

Wireshark provides context-sensitive pop-up menus.

Right-clicking different parts of the interface provides different options.

Important areas include:

- Packet List column header
- Packet List pane
- Packet Details pane
- Packet Bytes pane
- Packet Diagram pane


## Packet List Column Header

Right-clicking a column header provides options such as:

- Align Left
- Align Center
- Align Right
- Column Preferences
- Edit Column
- Resize To Contents
- Freeze Column
- Display as Values
- Display as Strings
- Display as Packet Details
- Show/Hide columns
- Remove Column

These are mainly interface/customization features.


## Packet List Pane

Right-clicking a packet provides useful analysis actions such as:

- Mark Packet
- Ignore Packet
- Set Time Reference
- Pin Row to Top
- Time Shift
- Packet Comment
- Apply as Filter
- Prepare as Filter
- Conversation Filter
- Colorize Conversation
- Follow
- Copy information
- Decode As
- Show Packet in New Window

Some of these become very useful during real investigations.


## Packet Details Pane

Right-clicking a protocol field can provide:

- Expand Subtrees
- Collapse Subtrees
- Apply as Column
- Apply as Filter
- Prepare as Filter
- Colorize with Filter
- Follow
- Copy Description
- Copy Fieldname
- Copy Value
- Copy As Filter
- Export Packet Bytes
- Decode As
- Go to Linked Packet
- Show Linked Packet in New Window

This makes the Packet Details pane an interactive analysis area rather than just a place to read information.


## Packet Bytes Pane

Right-clicking Packet Bytes provides options such as:

- Copy as Hex + ASCII
- Copy as Hex Dump
- Copy as Printable Text
- Copy as Hex Stream
- Copy as Raw Binary
- Copy as Escaped String
- Show as hexadecimal
- Show as bits
- Show text
- Show ASCII
- Show EBCDIC


## Packet Diagram Pane

The Packet Diagram context menu can:

- Show Field Values
- Save Diagram As an image
- Copy the diagram as a raster image

## Important Lesson

You do NOT need to memorize every context-menu option.

The important concept is:

> Right-clicking different parts of Wireshark gives tools related to the thing you clicked.


---

# 6.3 — Filtering Packets While Viewing

Wireshark has two filtering systems:

## Capture Filter

Used BEFORE/during capture.

Purpose:

    "What packets should Wireshark capture?"

## Display Filter

Used AFTER packets have been captured.

Purpose:

    "Which captured packets should Wireshark display?"

This distinction is extremely important.

Example:

    Capture contains:
    
    TCP
    UDP
    DNS
    ARP
    ICMP
    HTTP

Apply:

    tcp

Now only TCP packets are displayed.

The other packets are NOT deleted.

They remain in the capture file.

Clear the display filter and they become visible again.


## Simple Display Filter

    tcp

Meaning:

> Display packets containing TCP.


Another example:

    http.request

Meaning:

> Display packets containing the HTTP request field.


## Important Rule

Display filters change what you SEE.

They do not remove packets from the capture.


## Filter Workflow

    Capture
       ↓
    Many packets
       ↓
    Display filter
       ↓
    Smaller visible set
       ↓
    Analyze interesting packets


---

# 6.4 — Building Display Filter Expressions

Display filters are Wireshark's language for precisely selecting packets.

A filter can check:

- Protocol
- Field presence
- Field values
- Comparisons
- Multiple conditions
- Ranges
- Bytes
- Strings
- Time
- Other fields


## 6.4.1 — Display Filter Fields

A simple filter can be a protocol:

    tcp

    udp

    dns

    http

A field can also be used:

    http.request

A field represents a specific piece of information inside a packet.


---

# 6.4.2 — Comparing Values

Common comparison operators:

    ==      Equal
    !=      Not equal
    ===     Equal, all values
    !==     Not equal, any value
    >       Greater than
    <       Less than
    >=      Greater than or equal
    <=      Less than or equal
    contains
    matches


## Examples

    ip.addr == 192.168.0.1

    tcp.port == 443

    frame.len > 100

    http.request.uri contains "login"


## ANY vs ALL

This is important when a field can occur multiple times.

    ==

means equal / ANY matching value.

    ===

means equal / ALL values.

    !=

means not equal / ALL values.

    !==

means not equal / ANY value.


## English and C-like syntax

Wireshark supports forms such as:

    eq    ==
    ne    !=
    gt    >
    lt    <
    ge    >=
    le    <=

Example:

    ip.len le 1500

is equivalent to:

    ip.len <= 1500


---

# Display Filter Field Types

Different fields contain different types of values.

## Unsigned Integer

Whole number without a negative value.

Example:

    ip.len le 1500

Numbers can be represented in:

- Decimal
- Octal
- Hexadecimal
- Binary


## Signed Integer

Can contain positive and negative values.

Example concept:

    -10
    0
    10


## Boolean

Boolean values can be:

    True
    False

or:

    1
    0

Important:

A Boolean field being present does NOT necessarily mean its value is True.

For example:

    tcp.flags.syn

checks for the field.

To specifically require SYN to be set:

    tcp.flags.syn == 1

or:

    tcp.flags.syn == True


## Ethernet Address

Example:

    eth.dst == ff:ff:ff:ff:ff:ff


## IPv4 Address

Example:

    ip.addr == 192.168.0.1

CIDR can be used for subnet matching:

    ip.addr == 129.111.0.0/16


## IPv6 Address

Example:

    ipv6.addr == ::1


## Text String

Example:

    http.request.uri == "https://www.wireshark.org/"


## Escape Sequences

Strings can use escape sequences.

Important examples:

    \"    double quote
    \\    backslash
    \n    new line
    \t    horizontal tab
    \r    carriage return
    \xhh  hexadecimal character
    \uhhhh Unicode codepoint

Raw strings can be written using:

    r"..."

In a raw string, the backslash is treated as a literal character.


---

# Date and Time Fields

Absolute time fields can be compared using time strings.

Example:

    frame.time == "Sep 26, 2004 23:18:04.954975"

Another format:

    ntp.xmt ge "2020-07-04 12:34:56"

A date can also be used without specifying every time component:

    frame.time < "2022-01-01"


---

# 6.4.3 — Combining Expressions

Multiple conditions can be combined.

## AND

    and

or:

    &&

Example:

    ip.src == 10.0.0.5 and tcp.flags.syn == 1

Meaning:

> Source IP must be 10.0.0.5 AND SYN must be set.


## OR

    or

or:

    ||

Example:

    ip.src == 10.0.0.5 or ip.src == 192.168.1.1

Meaning:

> Match either source address.


## NOT

    not

or:

    !

Example:

    not arp

Meaning:

> Do not display ARP packets.


## XOR

    xor

or:

    ^^

XOR means one condition or the other, but not both.


## Parentheses

Parentheses can be used to group conditions.

Example:

    (tcp.port == 80 or tcp.port == 443)
    and ip.addr == 192.168.1.10


---

# 6.4.4 — Slice Operator

The slice operator allows Wireshark to select part of a field or protocol's bytes.

Conceptually:

    field[start:length]

Example:

    eth.src[0:3]

means:

> Select part of the Ethernet source address.

The operator can also use ranges and negative offsets.

Negative offsets count from the end.

Example:

    frame[-4:]

means:

> Select the last four bytes of the frame.


## Current Learning Priority

Understand:

> Slice = select a portion of a field/bytes.

Do not try to memorize complex slice syntax yet.


---

# 6.4.5 — Layer Operator

The layer operator is used when a field may occur at different protocol layers/instances.

It allows filtering based on a particular layer of a protocol field.

This is an advanced display-filter feature.

Current priority:

    Understand the purpose.
    Practice later when a real packet requires it.


---

# 6.4.6 — At Operator

The @ operator is related to accessing the raw representation of fields.

It is an advanced display-filter feature.

Current priority:

> Understand that Wireshark provides an operator for working with the raw representation of a field.

Do not spend study time memorizing it yet.


---

# 6.4.7 — Membership Operator

The membership operator is:

    in

It tests whether a field belongs to a set/range.

Example:

    http.request.method in {"HEAD", "GET"}

Meaning:

> Match HTTP requests whose method is HEAD or GET.


Another example:

    ip.addr in {10.0.0.5 .. 10.0.0.9}

Meaning:

> Match addresses within the specified range.


---

# 6.4.8 — Arithmetic Operators

Wireshark supports arithmetic operations on numeric fields.

Examples:

    +
    -
    *
    /
    %
    &

Example:

    frame.cap_len < {14 + ip.hdr_len + tcp.hdr_len}


## Meaning

Wireshark can perform calculations while evaluating a display filter.

This is useful for advanced analysis.

Current priority:

> Understand that display filters can perform arithmetic. Deep usage can come later.


---

# 6.4.9 — Functions

Wireshark provides functions that can transform or analyze field values.

Important examples:

    upper()
    lower()
    len()
    count()
    string()
    vals()
    dec()
    hex()
    float()
    double()
    max()
    min()
    abs()


## Examples

Convert text to lowercase:

    lower(http.server)

Find long HTTP request URIs:

    len(http.request.uri) > 100

Find packets containing more than two IP address fields:

    count(ip.addr) > 2


## Important idea

Functions allow the display filter to:

    Convert
    Count
    Measure
    Compare
    Transform


---

# 6.4.10 — Field References

A field reference has this general form:

    ${proto.field}

It reads a field value from the currently selected frame.

This allows dynamic filters.

Example:

    frame.time_relative >= ${frame.time_relative} - 300


## Mental Model

Normal filter:

    Compare against a fixed value.

Field reference:

    Get a value from the currently selected packet and use it in the filter.


This is an advanced feature.

It becomes more useful when building dynamic investigation filters.


---

# 6.4.11 — Implicit Type Conversions

Wireshark can sometimes automatically convert values between compatible types.

For example, some comparisons can convert:

    strings
    integers
    booleans
    byte arrays

according to the field and expression.

Current priority:

> Understand that Wireshark sometimes performs automatic type conversion.

Do not spend time memorizing every conversion rule.


---

# 6.4.12 — Sometimes Fields Change Names

Wireshark protocol fields can have names that change between protocol versions or dissectors.

Therefore:

> Do not assume a field name will always be identical across every protocol situation/version.

When a filter does not work as expected, use Wireshark's field information and filter tools to determine the correct field name.


---

# 6.4.13 — Some Protocol Names Can Be Ambiguous

Some values can potentially be interpreted as:

- Protocol names
- Numeric/hexadecimal values
- Byte arrays

Wireshark provides explicit syntax to remove ambiguity.

Examples include:

    .fc

for an explicit protocol name, and:

    :fc

for an explicit byte-array value.

This is an advanced syntax issue.

Current priority:

> Know that ambiguous filter values exist and Wireshark provides explicit syntax.


---

# 6.5 — Display Filter Expression Dialog

The Display Filter Expression dialog helps build filters without remembering every field name.

Basic workflow:

    Select Field
         ↓
    Select Relation
         ↓
    Enter Value
         ↓
    Build Filter


## Field Name

The dialog provides a tree of protocols and their filterable fields.

You can search for fields.

Example:

    tcp

or:

    tcp.*flag


## Relation

Examples include:

    is present
    ==
    !=
    >
    <
    >=
    <=
    contains
    matches
    in


## Is Present

This is different from a comparison.

It asks:

> Does this field exist in the packet?

Example:

    tcp.flags.syn

The field can exist even when its Boolean value is False.


## Value

The Value box provides the value to compare against.

Example:

    tcp.port == 443


## Predefined Values

Some fields have predefined values, similar to enumerated values.

## Search

Search can locate field names or descriptions.

Regular expressions are supported.

## Range

Can represent integer ranges such as:

    1-12

or:

    39-42,98-2000


## Main Purpose

The Expression dialog is a learning tool.

Instead of memorizing every field name:

    Search
      ↓
    Find field
      ↓
    Choose relation
      ↓
    Enter value
      ↓
    Let Wireshark build the expression


---

# 6.6 — Defining and Saving Filters

Saved filters are reusable display/capture filter entries.

They are useful when a filter is used repeatedly.

Example:

    Filter Name:
    My DNS Traffic

    Filter Expression:
    dns


## Important distinction

Filter name:

    My DNS Traffic

is only a convenient label.

The actual filter is:

    dns


## Main operations

    +
    Add filter

    Double-click
    Edit filter

    -
    Delete filter

    Copy
    Duplicate filter

    OK
    Save changes

    Cancel
    Close without saving changes


## Why save filters?

Instead of repeatedly typing:

    tcp.flags.syn == 1

you can save it and reuse it.


---

# 6.7 — Defining and Saving Filter Macros

A display filter macro is a reusable shortcut/template for a more complex display filter.

The important idea is:

    Saved Filter
        ↓
    Save a complete filter

    Macro
        ↓
    Save a filter template
    with placeholders


## Placeholders

Macros can use:

    $1
    $2
    $3
    ...

These represent values supplied when the macro is used.


## Example Concept

A macro might contain:

    ip.src == $1
    ip.dst == $2
    tcp.srcport == $3
    tcp.dstport == $4


Then values can be supplied later.

Conceptually:

    $1 → Source IP
    $2 → Destination IP
    $3 → Source Port
    $4 → Destination Port


## Macro Invocation

One supported form is:

    $macro_name(value1,value2,value3,value4)


## Important Difference

Saved filter:

    "Reuse this exact filter."

Macro:

    "Reuse this filter logic with different input values."


## Important limitation

Display filter macros can be used with saved display filters and filter buttons, but not capture filters.


## Current Priority

Understand:

> Macro = reusable filter template.

Do not spend time memorizing every macro syntax.


---

# 6.8 — Finding Packets

The Find Packet feature helps locate packets in a capture.

You can search using:

1. Display filter
2. Hexadecimal value
3. String
4. Regular expression


## Display Filter Search

Example:

    ip.src==192.168.0.1 and tcp.flags.syn==1

Wireshark searches for a packet matching the condition.


## Hexadecimal Value

Searches packet data for a specific byte sequence.

Example:

    ef:bb:bf


## String

Searches packet data for text.


## Regular Expression

Searches packet data using a regular-expression pattern.


## Find in Selected Packet

If exactly one packet is selected, Wireshark can search inside that packet's protocol tree.

This is different from searching the entire capture.

Conceptually:

    Entire capture
         ↓
    Find Packet

versus:

    One selected packet
         ↓
    Find in selected Packet


## Important Difference

Display Filter:

    "Show packets matching this condition."

Find Packet:

    "Find/jump to the next packet matching this condition."


---

# 6.9 — Go To A Specific Packet

Navigation features allow you to move around the packet list.

## Go Back

Moves backward through packet navigation history.

Similar to browser Back.


## Go Forward

Moves forward through packet navigation history.


## Go to Packet

Enter a packet number.

Example:

    Packet: 42

Wireshark jumps to packet 42.


## Go to Corresponding Packet

Some fields point to another packet.

For example:

    DNS response
        ↓
    corresponding DNS request

Wireshark can jump to the corresponding packet.


## First Packet

Jumps to the first currently displayed packet.


## Last Packet

Jumps to the last currently displayed packet.


## Important Difference

    Find Packet
        ↓
    Search for a packet

    Go to Packet
        ↓
    Navigate directly to a packet number


---

# 6.10 — Marking Packets

Marking is used to identify important packets.

A marked packet is shown with a black background regardless of packet coloring rules.

Example workflow:

    Packet 42
       ↓
    Important
       ↓
    Mark Packet


## Why mark packets?

In a large capture, you may find important packets such as:

- Connection start
- DNS request
- Suspicious packet
- Authentication event
- HTTP request
- Important response

You can mark them so you can find/use them later.


## Important

Mark information is NOT stored permanently in the capture file.

It is lost when the capture file is closed.


## Marking Options

    Mark/Unmark Selected

    Mark All Displayed

    Unmark All Displayed


## Marked Packets and Export

Marked packets can be selected as a packet range when saving/exporting/printing.


---

# 6.11 — Ignoring Packets

Ignoring a packet tells Wireshark to behave as if the packet does not exist for relevant dissection/analysis purposes.

Ignored packets are shown with:

- White background
- Grey foreground

regardless of coloring rules.


## Important Difference

Marking:

    "This packet is important."

Ignoring:

    "Treat this packet as ignored."


## Ignore Options

    Ignore/Unignore Selected

    Ignore All Displayed

    Unignore All Displayed


## Important

Ignore information is NOT permanently stored in the capture file.

It is lost when the capture file is closed.


---

# Mark vs Ignore

This distinction is important.

    MARK

    "Remember this packet.
     It is important."

    IGNORE

    "Treat this packet as ignored."


A marked packet is useful for investigation and later export/printing.

An ignored packet is excluded from relevant processing as an ignored packet.


---

# 6.12 — Time Display Formats and Time References

Every captured packet receives a timestamp.

The timestamp is saved with the capture and can be used during later analysis.


## Time Display Format

Time display format answers:

> How should Wireshark SHOW the packet timestamp?


## Available Formats

### 1. Date and Time of Day

Example:

    2026-09-15 10:00:00.123456

Shows:

    Date + Time


### 2. Time of Day

Example:

    10:00:00.123456

Shows:

    Time only


### 3. Seconds Since First Captured Packet

Example:

    Packet 1 → 0.000000
    Packet 2 → 2.000000
    Packet 3 → 5.000000

Question:

> How many seconds have passed since the starting point?


### 4. Seconds Since Previous Captured Packet

Example:

    Packet 1 → 0
    Packet 2 → 2
    Packet 3 → 3

Question:

> How much time passed since the previous captured packet?


### 5. Seconds Since Previous Displayed Packet

This considers only currently displayed packets.

Example:

    Capture:
    Packet 1
    Packet 2
    Packet 3
    Packet 4

After filtering:

    Packet 1
    Packet 4

Packet 4's previous DISPLAYED packet is Packet 1.


### 6. Seconds Since Epoch

Epoch begins at:

    January 1, 1970 00:00:00 UTC

The timestamp is represented as seconds since that point.


---

# Timestamp Precision

Precision controls how many decimal places are displayed.

Possible settings include:

    Seconds
    Tenths
    Hundredths
    Milliseconds
    Microseconds
    Nanoseconds


Example:

Actual:

    1.123456789


Seconds:

    1


Milliseconds:

    1.123


Microseconds:

    1.123456


Nanoseconds:

    1.123456789


## Important Difference

    FORMAT
    ↓
    What does the timestamp represent?

    PRECISION
    ↓
    How much detail is displayed?


---

# 6.12.1 — Packet Time Referencing

A Time Reference lets you choose a packet as a new starting point for relative-time calculations.

Example:

    Packet 100 → TCP SYN
    Packet 101 → SYN/ACK
    Packet 102 → ACK
    Packet 103 → HTTP request

Set Packet 100 as the Time Reference.

Now:

    Packet 100 → 0.000 sec
    Packet 101 → 0.002 sec
    Packet 102 → 0.003 sec
    Packet 103 → 0.010 sec


## Why is this useful?

It lets you analyze timing relative to an important event.

For example:

    Connection starts
          ↓
       0 sec
          ↓
    SYN/ACK
          ↓
    HTTP request
          ↓
    HTTP response


## Time Reference Marker

A time-referenced packet is shown with:

    *REF*

in the Time column.


## Important

Time references are not permanently saved.

They are lost when the capture file is closed.


## Multiple Time References

Multiple packets can be used as time references.

Subsequent relative-time calculations use the most recent time reference.


## Display Filters

Time-referenced packets remain visible in the Packet List even when a display filter would normally hide them.


---

# 6.13 — Time Shifting Packets

Time Shift is different from Time Reference.

## Time Reference

Changes the starting point for relative-time calculations.

    "Start counting time from this packet."


## Time Shift

Adjusts the timestamps themselves.

    "The timestamps are wrong.
     Correct them."


---

# Why Time Shift Is Needed

Possible reasons include:

- Incorrect capture-machine clock
- Incorrect timezone handling
- Synchronizing timestamps between different captures
- Correcting clock skew
- Correcting clock drift

This can be useful before merging captures from different machines.


---

# Shift All Packets

A fixed offset can be applied to all packet timestamps.

Example:

    Shift all packets by +01:00:00

Every packet moves by one hour.


## One Known Packet

You can specify:

    Packet number
    Correct absolute time

Wireshark calculates a fixed offset.

This can correct clock skew.


## Two Known Packets

You can specify two packets with known correct times.

Wireshark calculates the correction linearly across the capture.

This can help correct clock drift.


## NTP / PTP

Packets containing accurate timing information, such as NTP or PTP, can be useful when the correct timing of specific packets is known.


## Undo All Shifts

Removes all unsaved time shifts.


## Important

Time shifts apply to:

- Normal packets
- Ignored packets
- Packets hidden by the current display filter

Wireshark does not provide a method to shift timestamps of only one selected packet.


## frame.offset_shift

The currently applied time shift can be viewed through:

    frame.offset_shift


## Saving

After a time shift, the capture has unsaved changes.

An asterisk appears beside the capture filename.

Saving the capture writes the corrected timestamps to the capture file.


---

# Time Reference vs Time Shift

This is one of the most important distinctions in Chapter 6.

| Feature | Time Reference | Time Shift |
|---|---|---|
| Purpose | Change relative-time starting point | Correct packet timestamps |
| Changes actual timestamp? | No | Yes |
| Example | "Start timing from Packet 42" | "Move all timestamps +1 hour" |
| Permanent? | No | Can be saved |
| Used for | Packet timing analysis | Timestamp correction |

Simple memory:

    Time Reference
        ↓
    CHANGE STARTING POINT

    Time Shift
        ↓
    CHANGE TIMESTAMP


---

# Chapter 6 — Complete Mental Model

Chapter 6 can be understood as one workflow:

    CAPTURE FILE
         │
         ↓
    ┌───────────────────┐
    │ View packets       │
    └─────────┬─────────┘
              ↓
    ┌───────────────────┐
    │ Packet List        │
    └─────────┬─────────┘
              ↓
    ┌───────────────────┐
    │ Packet Details     │
    └─────────┬─────────┘
              ↓
    ┌───────────────────┐
    │ Packet Bytes       │
    └─────────┬─────────┘
              ↓
    ┌───────────────────┐
    │ Display Filters    │
    └─────────┬─────────┘
              ↓
    ┌───────────────────┐
    │ Find / Navigate    │
    └─────────┬─────────┘
              ↓
    ┌───────────────────┐
    │ Mark / Ignore      │
    └─────────┬─────────┘
              ↓
    ┌───────────────────┐
    │ Analyze timing     │
    └─────────┬─────────┘
              ↓
    ┌───────────────────┐
    │ Time Reference     │
    └─────────┬─────────┘
              ↓
    ┌───────────────────┐
    │ Time Shift         │
    └───────────────────┘


# Most Important Skills From Chapter 6

## Tier 1 — MUST KNOW

These are the skills we will actively practice:

- Packet List
- Packet Details
- Packet Bytes
- Protocol tree
- Selecting fields
- Understanding highlighted bytes
- Capture Filter vs Display Filter
- Basic display filters
- `tcp`
- `udp`
- `dns`
- `http`
- `ip.addr`
- `ip.src`
- `ip.dst`
- `tcp.port`
- `tcp.flags`
- `==`
- `!=`
- `>`
- `<`
- `>=`
- `<=`
- `and`
- `or`
- `not`
- `contains`
- Find Packet
- Go to Packet
- Go to Corresponding Packet
- Basic timestamp formats
- Relative time
- Time Reference


# Tier 2 — SHOULD KNOW

Understand the purpose and practice later:

- Display Filter Expression dialog
- Saved filters
- Filter buttons
- Marking packets
- Ignoring packets
- String searching
- Hex searching
- Follow-related context menu features
- Conversation filters
- Apply as Filter
- Prepare as Filter
- Apply as Column
- Colorize with Filter
- Time Shift


# Tier 3 — REFERENCE / ADVANCED

You do NOT need to memorize these now:

- Slice operator
- Layer operator
- At operator
- Membership operator
- Arithmetic expressions
- Advanced functions
- Field references
- Implicit type conversions
- Ambiguous protocol/value names
- Complex filter macros
- Complex regular expressions
- Advanced time-shift correction


# Practical Wireshark Workflow After Chapter 6

The knowledge from Chapter 6 should now become practical.

A basic investigation can look like:

    1. Open/capture traffic
          ↓
    2. Look at Packet List
          ↓
    3. Select interesting packet
          ↓
    4. Inspect Packet Details
          ↓
    5. Inspect Packet Bytes
          ↓
    6. Apply display filter
          ↓
    7. Narrow the traffic
          ↓
    8. Find important packets
          ↓
    9. Follow related traffic
          ↓
    10. Check timing
          ↓
    11. Mark important packets
          ↓
    12. Build an understanding of what happened


# Chapter 6 — Core Lessons

1. A packet is not just one line in the Packet List.

2. The Packet List gives the summary.

3. Packet Details explains the protocol structure.

4. Packet Bytes show the underlying packet data.

5. Selecting a field in Packet Details can highlight its corresponding bytes.

6. Display filters hide packets from the current view but do not remove them from the capture.

7. Capture filters and display filters serve different purposes.

8. Display filters can become very powerful and complex.

9. The Expression dialog helps discover fields and construct filters.

10. Saved filters reduce repeated typing.

11. Macros provide reusable filter templates with input values.

12. Find Packet searches for packets.

13. Go to Packet navigates directly to a packet number.

14. Marking identifies packets as important.

15. Ignoring tells Wireshark to treat packets as ignored.

16. Timestamp format controls how packet time is displayed.

17. Timestamp precision controls how much timing detail is displayed.

18. Time Reference creates a new starting point for relative timing.

19. Time Shift adjusts packet timestamps.

20. Time Reference and Time Shift are NOT the same thing.


# Final Chapter 6 Mental Model

    Wireshark
         │
         ↓
    "I have captured packets."
         │
         ↓
    VIEW
         │
         ├── Packet List
         ├── Packet Details
         └── Packet Bytes
         │
         ↓
    FILTER
         │
         ├── Protocol
         ├── Field
         ├── Value
         └── Conditions
         │
         ↓
    FIND
         │
         ├── Display Filter
         ├── String
         ├── Hex
         └── Regex
         │
         ↓
    NAVIGATE
         │
         ├── Go to Packet
         ├── Back
         ├── Forward
         └── Corresponding Packet
         │
         ↓
    ORGANIZE
         │
         ├── Mark
         └── Ignore
         │
         ↓
    TIMING
         │
         ├── Timestamp formats
         ├── Relative time
         ├── Time Reference
         └── Time Shift
         │
         ↓
    ACTUAL PACKET ANALYSIS
