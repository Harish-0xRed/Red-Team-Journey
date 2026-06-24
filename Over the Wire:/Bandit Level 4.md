# Bandit Level 4 Notes

## Objective

Find the password for the next level stored in the only human-readable file inside the inhere directory.

## Website

OverTheWire Bandit

## Level Goal

The password for the next level is stored in the only human-readable file in the inhere directory.

## Commands Used

```bash
ls
cd inhere
file -- -file00
file -- -file01
file -- -file02
file -- -file03
file -- -file04
file -- -file05
file -- -file06
file -- -file07
file -- -file08
file -- -file09

cat -- -file07
```

## Command Breakdown

# ls

Lists files and directories.

# cd

Changes the current directory.

# file

Identifies the type of a file.

# cat

Displays the contents of a file.

# --

Stops option processing and treats the following text as a filename.

## Investigation Process

1. Logged into bandit4.
2. Navigated to the inhere directory.
3. Listed all files.
4. Observed multiple files named:

```text
-file00
-file01
...
-file09
```

5. Used the file command to identify file types.
6. Discovered that:

```text
-file07: ASCII text
```

7. Read the contents of the ASCII text file using cat.

## What is ASCII Text?

ASCII text is human-readable text that can be displayed and read directly in the terminal.

Example:

```text
Hello World
password123
Linux
```

## Skills Learned

* Using the file command
* Identifying file types
* Understanding human-readable files
* Recognizing ASCII text
* Investigating files before opening them
* Using "--" with filenames beginning with "-"

## Real-World Relevance

Linux administrators, SOC analysts, and security professionals frequently investigate unknown files. The file command helps identify file types before opening or executing them.

## Level Completion

Level 4 → Successfully identified the only human-readable file and retrieved the password for Level 5.

## Key Takeaway

Do not guess.

Investigate first.

Use the file command to identify file types before interacting with unknown files.
