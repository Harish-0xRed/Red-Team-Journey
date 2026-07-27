# 📒 Linux Essentials Complete Study Guide (Chapters 1–18)

---

## 📒 Chapter 1: Introduction to Linux

* **Linux Kernel:** Linux is the core kernel that manages CPU, memory, storage, and processes.
* **GNU/Linux:** GNU provides tools and commands, while Linux provides the kernel; together they form GNU/Linux.
* **Key History:** Inspired by UNIX; Linus Torvalds created the Linux Kernel in 1991, and Richard Stallman founded the GNU Project.
* **Open Source:** Source code can be viewed, modified, and shared freely.
* **Distributions:** Bundles containing the Linux Kernel, GNU tools, and software applications (e.g., Ubuntu, Debian, Kali Linux).
* **Interfaces:** GUI (graphical) vs. CLI (command line interface—preferred in cybersecurity for speed, precision, and automation).

---

## 📒 Chapter 2: Operating Systems

* **OS Role:** Manages hardware, software, system resources, and multitasking.
* **Desktop vs Server:** Desktops rely on a GUI for interactive daily use; Servers rely on a CLI to minimize resource consumption.
* **Virtualization & Cloud:** Running guest virtual machines via a hypervisor (e.g., VirtualBox) or renting remote cloud infrastructure (AWS, Azure, GCP).
* **Releases:** Stable releases (tested for production) vs Beta releases (contains new features, higher risk).
* **Package Managers:** OS-specific package management tools (e.g., `apt` for Debian/Kali).
* **Distribution Families:**
  * **Debian Family:** Debian, Ubuntu, Linux Mint, Kali Linux
  * **Red Hat Family:** Red Hat Enterprise Linux (RHEL), Fedora, CentOS
  * **SUSE Family:** openSUSE, SLES

---

## 📒 Chapter 3: Linux Ecosystem, Security, and Cloud

* **Applications:** Web servers (Apache, NGINX), Databases (MariaDB, PostgreSQL), File Sharing (Samba, NFS, Netatalk).
* **Shells & Editors:** Shells translate user input for the kernel (Bash, Zsh); text editors range from beginner-friendly (Nano) to advanced (Vim).
* **Development Languages:** Compiled languages (C, C++) vs Interpreted languages (Python, PHP, Perl, Ruby).
* **Security & Privacy:** Root user security, SSH for remote logins, system firewalls (`iptables`, `ufw`), VPNs, and HTTPS.
* **Cloud & Containers:** Host vs Guest systems, Hypervisors, and container technologies like Docker and Kubernetes.

---

## 📒 Chapter 4: Open Source Software and Licensing

* **Source Code vs Binary:** Source code is human-readable; binaries are machine-executable compiled files.
* **GPL (Copyleft):** If you modify and redistribute copyleft software, you must release your modified source code under the same license.
* **Permissive Licenses:** (BSD, MIT, Apache) Allow commercial use and closed-source modifications without forcing open-source redistribution.
* **Organizations:** FSF (Free Software Foundation – freedom-focused) vs OSI (Open Source Initiative – business and development-focused).
* **Creative Commons (CC):** Used for non-software media and content:
  * **BY:** Attribution required
  * **SA:** ShareAlike under identical terms
  * **NC:** NonCommercial use only
  * **ND:** NoDerivatives (no modifications allowed)
  * **CC0:** Public Domain equivalent

---

## 📒 Chapter 5: Command Line Interface Fundamentals

* **Command Syntax:** `command [options] [arguments]`
* **Prompt Signs:** `$` = Normal user, `#` = Root user (administrator).
* **Environment vs Local Variables:** System-wide (`PATH`, `HOME`, `HISTSIZE`) vs local variables confined to the current shell.
* **Command Operations:**
  * `history` / `!!` / `!n` $\rightarrow$ View and re-execute previous history items.
  * `type` / `which` $\rightarrow$ Identify built-in shell commands vs external binary locations.
  * `alias` $\rightarrow$ Create custom command shortcuts.
* **Control Operators:**
  * `;` $\rightarrow$ Always run the next command.
  * `&&` $\rightarrow$ Run the next command ONLY if the previous command succeeds.
  * `||` $\rightarrow$ Run the next command ONLY if the previous command fails.

---

## 📒 Chapter 6: Finding Help in Linux

* **`man` Pages:** Open the full manual page (`man command`). Exit using `q`.
  * `man -f` or `whatis` $\rightarrow$ Display short manual description.
  * `man -k` or `apropos` $\rightarrow$ Search manuals by keyword.
* **`info`:** Comprehensive, textbook-style documentation.
* **Locating Files & Executables:**
  * `which` $\rightarrow$ Displays the executable binary path found in your `$PATH`.
  * `whereis` $\rightarrow$ Locates binary, source code, and manual page files.
  * `locate` $\rightarrow$ Performs fast index-based file searches (updated via `updatedb`).

---

## 📒 Chapter 7: Navigating the Filesystem

* **Root Directory:** Top of the Linux filesystem hierarchy (`/`).
* **Shortcuts:** `~` (Current user home directory), `.` (Current directory), `..` (Parent directory).
* **Navigation & Directory Inspection:**
  * `pwd` $\rightarrow$ Print working directory path.
  * `cd` $\rightarrow$ Change directory.
  * `ls -la` $\rightarrow$ Long listing showing all hidden files (files starting with `.`).
  * `ls -lh` $\rightarrow$ Displays human-readable file sizes (K, M, G).
  * `ls -R` $\rightarrow$ Performs a recursive listing of subdirectories.

---

## 📒 Chapter 8: Managing Files and Directories

* **File Operations:**
  * `touch` $\rightarrow$ Create an empty file or update existing file timestamps.
  * `cp` / `cp -r` $\rightarrow$ Copy files / copy directories recursively.
  * `mv` $\rightarrow$ Move or rename files and directories.
  * `rm` / `rm -r` / `rmdir` $\rightarrow$ Delete files / recursive folder deletion / delete empty directories only.
* **Wildcards (Globbing):**
  * `*` $\rightarrow$ Matches zero or more characters.
  * `?` $\rightarrow$ Matches exactly one character.
  * `[abc]` / `[a-z]` $\rightarrow$ Matches one character from a specified set or range.

---

## 📒 Chapter 9: Compression and Archiving

* **Compression Tools:**
  * `gzip` / `gunzip` $\rightarrow$ `.gz` format
  * `bzip2` / `bunzip2` $\rightarrow$ `.bz2` format
  * `xz` / `unxz` $\rightarrow$ `.xz` format
* **Tape Archive (`tar`):**
  * `tar -czf archive.tar.gz /path` $\rightarrow$ Create a `gzip` compressed archive.
  * `tar -xzf archive.tar.gz` $\rightarrow$ Extract a `gzip` compressed archive.
  * **Flags:** `-c` (create), `-x` (extract), `-t` (list), `-z` (`gzip`), `-j` (`bzip2`), `-J` (`xz`), `-f` (filename).

---

## 📒 Chapter 10: Working with Text

* **Viewing Text:** `cat` (entire file), `less` (scrollable), `head` (first lines), `tail` (last lines), `tail -f` (live log monitoring).
* **Redirection & Pipes:**
  * `|` $\rightarrow$ Pass stdout of one command as stdin to another.
  * `>` / `>>` $\rightarrow$ Redirect stdout to a file (overwrite / append).
  * `2>` / `&>` $\rightarrow$ Redirect stderr only / redirect stdout and stderr combined.
* **Text Processing & Filtering:**
  * `sort` $\rightarrow$ Sort text lines alphabetically, numerically (`-n`), or reversed (`-r`).
  * `wc -l` $\rightarrow$ Count total lines in input text.
  * `cut -d, -f2` $\rightarrow$ Extract specified column fields using a delimiter.
  * `grep` $\rightarrow$ Search lines matching patterns (`-i` ignore case, `-v` invert match, `-c` count).

---

## 📒 Chapter 11: Basic Scripting

* **Shebang:** `#!/bin/bash` placed on line 1 specifies the execution interpreter.
* **Execution:** Add execute permissions using `chmod +x script.sh`, then run with `./script.sh`.
* **Variables & Positional Parameters:**
  * Declaration: `NAME="Harish"` (no spaces around `=`).
  * Accessing: `echo $NAME`
  * Parameters: `$0` (script name), `$1` (first argument), `$2` (second argument).
  * Exit Status: `$?` (`0` indicates success; non-zero indicates an error).
* **Flow Control:** `if ... then ... elif ... else ... fi`, `case` statements, `for` loops, and `while` loops.

---

## 📒 Chapter 12: Computer Hardware

* **System Information Commands:**
  * `arch` / `lscpu` $\rightarrow$ View CPU architecture and CPU core details.
  * `free -m` $\rightarrow$ View system RAM and Swap utilization in megabytes.
* **Hardware Inspection:**
  * `lspci` $\rightarrow$ List connected PCI devices (GPU, NIC, Sound cards).
  * `lsusb` $\rightarrow$ List connected USB devices.
* **Disk Drive Naming:**
  * Physical Disks: `/dev/sda`, `/dev/sdb`
  * Partition Labels: `/dev/sda1`, `/dev/sda2`
  * Inspection: `sudo fdisk -l`

---

## 📒 Chapter 13: System & Process Management

* **Kernel & Process Basics:**
  * The initial system process (`init` or `systemd`) always assigns **PID 1**.
  * `/proc` $\rightarrow$ Virtual memory-backed directory containing kernel and process status.
* **Process Inspection Commands:**
  * `ps aux` / `ps -ef` $\rightarrow$ Displays a static process snapshot.
  * `top` $\rightarrow$ Displays a dynamic real-time system resource and process monitor.
* **Filesystem Hierarchy Standard (FHS):**
  * `/etc` $\rightarrow$ System configuration files
  * `/var/log` $\rightarrow$ System and application logs
  * `/home` $\rightarrow$ Normal user personal data
  * `/tmp` $\rightarrow$ Temporary volatile storage

---

## 📒 Chapter 14: Networking in Linux

* **Network Utilities:**
  * `ip addr` / `ifconfig` $\rightarrow$ Display network interfaces and IP configurations.
  * `ip route` / `route` $\rightarrow$ Display IP routing tables and default gateways.
  * `ss` / `netstat` $\rightarrow$ Display listening ports and network socket connections.
  * `ping` $\rightarrow$ Test network reachability using ICMP echo requests.
  * `dig` / `host` $\rightarrow$ Query DNS records (`dig` provides detailed output).
  * `ssh` $\rightarrow$ Establish encrypted remote administrative sessions.
* **Configuration Files:**
  * `/etc/resolv.conf` $\rightarrow$ Configures system DNS nameservers.
  * `/etc/hosts` $\rightarrow$ Static local mapping of IP addresses to hostnames.

---

## 📒 Chapter 15: Group and User Security

* **User Security:** Prevents routine admin tasks under root. Privilege escalation is managed via `sudo`.
* **Switching Accounts:**
  * `su -` $\rightarrow$ Switch user account loading a full target login shell environment.
  * `sudo` $\rightarrow$ Execute a single command with root privileges; logs user actions.
* **Account Files:**
  * `/etc/passwd` $\rightarrow$ User metadata (UID, primary GID, home directory, login shell).
  * `/etc/shadow` $\rightarrow$ Contains password hashes and password expiration settings.
  * `/etc/group` $\rightarrow$ Group metadata and secondary group membership lists.
* **Tracking Activity:** `id`, `who`, `w`, `last`.

---

## 📒 Chapter 16: Creating Users and Groups

* **Group Management:**
  * `groupadd -r groupname` $\rightarrow$ Add a system group with a low GID.
  * `groupmod` / `groupdel` $\rightarrow$ Modify GID or name / delete group (if not primary to a user).
* **User Management:**
  * `useradd -u 1009 -g users -G sales,research -m -c "Jane Doe" jane`
  * `usermod -aG sales jane` $\rightarrow$ Safely append supplementary group membership.
  * `userdel -r username` $\rightarrow$ Delete user account, home directory, and mail spool.
* **Password Aging & Policy:**
  * `passwd username` $\rightarrow$ Set or change password.
  * `chage -M 60 -W 7 username` $\rightarrow$ Configure password expiration and warning periods.

---

## 📒 Chapter 17: Ownership and Permissions

* **File Ownership:** Every file has a User Owner and a Group Owner.
* **Managing Ownership:**
  * `chown user:group file` $\rightarrow$ Change both User and Group owner.
  * `chgrp group file` $\rightarrow$ Change Group owner only.
* **Permission Types:** Read (`r` = 4), Write (`w` = 2), Execute (`x` = 1).
* **Permission Evaluation Rules:**
  1. Linux checks directory execute (`x`) permissions for every directory in the path first.
  2. Evaluates User Owner permissions if the user matches.
  3. Evaluates Group Owner permissions if the group matches.
  4. Evaluates Others permissions.
  * *Note:* Linux applies **only the first matching permission group** and never combines sets.
* **Modifying Permissions (`chmod`):**
  * Symbolic: `chmod u+x,g+w,o-r file`
  * Numeric: `chmod 754 file` (`rwx` for Owner, `r-x` for Group, `r--` for Others).
* **Default Permissions (`umask`):**
  * Subtracted from `666` (files) or `777` (directories).
  * Example: A `umask` of `027` sets new file permissions to `640` (`rw-r-----`) and directory permissions to `750` (`rwxr-x---`).

---

## 📒 Chapter 18: Special Permissions

### 18.2 Setuid (Set User ID)
* Executes binaries with the permissions of the **file owner** rather than the user running it (e.g., `/usr/bin/passwd`).
* **Symbolic:** `chmod u+s file` | **Numeric:** `chmod 4775 file`
* Indicated by `s` in the owner's execute position (`S` if execute permission is not set).

### 18.3 Setgid (Set Group ID)
* Executes binaries using the privileges of the **group owner**.
* Applied to directories, forces newly created files inside to inherit the directory's group owner.
* **Symbolic:** `chmod g+s dir` | **Numeric:** `chmod 2775 dir`
* Indicated by `s` in the group execute position.

### 18.4 Sticky Bit
* Applied to shared directories (e.g., `/tmp`, `/var/tmp`).
* Allows all users to create files, but prevents users from deleting files owned by others.
* **Symbolic:** `chmod o+t dir` | **Numeric:** `chmod 1775 dir`
* Indicated by `t` in the others execute position (`T` if execute permission is not set).

| Special Permission | Numeric Bit | Target Scope | Key Effect |
| :--- | :--- | :--- | :--- |
| **Setuid** | `4000` | Executable File | Run file with owner's permissions |
| **Setgid** | `2000` | File / Directory | Run file with group permissions / Inherit directory group |
| **Sticky Bit** | `1000` | Directory | Prevent non-owners from deleting files in shared dir |
