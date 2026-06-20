# Bandit Level 2

## Goal

Read a file named:

```text
--spaces in this filename--
```

---

## What This Level Teaches

### 1. Spaces in Filenames

Linux uses spaces to separate arguments.

Example:

```bash
cat my file
```

Linux thinks:

```text
cat
my
file
```

(2 arguments)

Not:

```text
my file
```

(1 filename)

---

## How to Handle Spaces

### Method 1: Quotes

```bash
cat "my file"
```

### Method 2: Backslash

```bash
cat my\ file
```

Both mean:

```text
my file
```

---

## Second Problem

The filename starts with:

```text
--
```

Linux may think it is a command option.

Example:

```bash
cat --help
```

shows help.

---

## Solution

Use:

```bash
cat -- "--spaces in this filename--"
```

---

## Meaning of `--`

```bash
--
```

means:

```text
Stop reading options.
Everything after this is a filename.
```

---

## Commands Used

```bash
ls
cat
```

---

## Key Lessons

Spaces split arguments
Quotes keep text together
`\` escapes spaces
`--` stops option processing
Some filenames can look like command options

---

## One-Line Memory Note

```text
Quotes/Backslash handle spaces.
-- tells Linux "this is a filename, not an option".
```
