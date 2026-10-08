# Python Learning Notes: for Loops, range() and Modules

Covers the `for` loop, `range()` (closed-open ranges and its three arguments), how `for` relates to `while`, and importing modules.

<a id="contents"></a>

## Contents

1. [Part 1: for Loops and range()](#part-1)
2. [Part 2: Closed-Open Range](#part-2)
3. [Part 3: for vs while](#part-3)
4. [Part 4: Arguments to range()](#part-4)
5. [Part 5: Importing Modules](#part-5)

---

<a id="part-1"></a>

```text
═══════════════════════ PART 1 : START ═══════════════════════
```

# Part 1: for Loops and range()

`for` loops are extremely common in automation and, eventually, in 0xRecon.

You already know the `while` loop:

> Keep going while this condition is `True`.

A `for` loop is useful when you want:

> Run a block a specific number of times.

The book introduces it with:

```python
for i in range(5):
    print('On this iteration, i is set to ' + str(i))
```

## Break Down the Syntax

```python
for i in range(5):
```

There are several pieces:

```text
for
 ↓
keyword

i
 ↓
variable

in
 ↓
keyword

range(5)
 ↓
values to loop through

:
 ↓
block is coming
```

Then:

```python
    print(...)
```

is the `for` block.

## What Does `range(5)` Give Us?

This is the most important part. `range(5)` produces values starting from:

```text
0
1
2
3
4
```

It does **not** include 5. So `range(5)` means:

```text
0 → 1 → 2 → 3 → 4
```

That's 5 values.

## What Happens to `i`?

Look at:

```python
for i in range(5):
```

Python takes each value from `range(5)` and assigns it to `i`.

**Iteration 1**

```text
i = 0
```

Then the block runs `print(...)`. Output:

```text
On this iteration, i is set to 0
```

**Iteration 2**

```text
i = 1
```

Output:

```text
On this iteration, i is set to 1
```

**Iteration 3:** `i = 2`

**Iteration 4:** `i = 3`

**Iteration 5:** `i = 4`

Then there are no more values. END LOOP.

## Compare `while` and `for`

### `while`

You had to manage the counter yourself:

```python
spam = 0
while spam < 5:
    print('Hello')
    spam = spam + 1
```

Notice `spam = spam + 1`: you manually change the counter.

### `for`

Python handles the next value:

```python
for i in range(5):
    print('Hello')
```

You don't need `i = i + 1`. The `for` loop moves to the next value automatically.

```text
while:
"Keep going while this condition is True."

for:
"Give me each value from this sequence."
```

## The Gauss Example

The book gives:

```python
total = 0
for num in range(101):
    total = total + num
print(total)
```

Initially:

```text
total = 0
```

Then:

```text
num = 0 → total = 0 + 0  → 0
num = 1 → total = 0 + 1  → 1
num = 2 → total = 1 + 2  → 3
num = 3 → total = 3 + 3  → 6
...
```

It continues until `num = 100`, because `range(101)` gives 0 through 100. Finally:

```text
total = 5050
```

## One Very Important Pattern

You'll see this pattern a lot in Python:

```python
total = 0
for number in range(...):
    total = total + number
```

The idea:

```text
Start with 0
     ↓
take a value
     ↓
add it to total
     ↓
take next value
     ↓
add it
     ↓
repeat
```

This is called **keeping a running total**. You don't need to memorize the name right now. Just understand the behavior.

## Connection to 0xRecon

Imagine eventually you have:

```python
ports = [22, 80, 443, 8080]
```

You will want to do something like:

```python
for port in ports:
    ...
```

Conceptually:

```text
port = 22
   ↓
check it

port = 80
   ↓
check it

port = 443
   ↓
check it

port = 8080
   ↓
check it
```

That's one reason `for` loops are important for your project. But don't jump into sockets yet. We're learning the loop itself first.

## Your Challenge

Don't run this yet:

```python
total = 0
for num in range(5):
    total = total + num
print(total)
```

Trace it exactly like you did with `while`:

```text
Starting:
total = ?

Iteration 1:
num = ?
total = ?

Iteration 2:
num = ?
total = ?

Iteration 3:
num = ?
total = ?

Iteration 4:
num = ?
total = ?

Iteration 5:
num = ?
total = ?

Final:
total = ?
```

Think first, don't run it.

```text
════════════════════════ PART 1 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 2](#part-2)

---

<a id="part-2"></a>

```text
═══════════════════════ PART 2 : START ═══════════════════════
```

# Part 2: Closed-Open Range

This part explains why Python uses the "up to but not including" rule for `range()`. Don't think:

> "Python randomly decided to exclude the last number."

There is a useful reason behind it.

## Closed-Open Range

The book calls this **"closed, open"**. For a range like `0, 10`, the starting number is included, but the ending number is excluded. So:

```text
0, 1, 2, 3, 4, 5, 6, 7, 8, 9
```

Not:

```text
0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
```

You can visualize it like:

```text
START                              END
  ↓                                 ↓
  0 ─── 1 ─── 2 ─── ... ─── 9 ─── 10
  ●                                  ○
 included                         excluded
```

## Why Is This Useful?

The biggest advantage the book gives is **easy range size calculation**. Suppose `0, 10`. Because the ending number is excluded:

```text
10 - 0 = 10
```

So there are exactly 10 numbers:

```text
0 1 2 3 4 5 6 7 8 9
←──── 10 numbers ────→
```

Very simple.

### Compare with including both ends

Suppose we wanted 0 through 9 with both ends included. We would calculate:

```text
9 - 0 + 1 = 10
```

That extra `+1` makes the calculation slightly more complicated and creates more opportunities for **off-by-one errors**.

## No Overlap, No Gap

Look at two ranges:

```text
0, 10
10, 20
```

The first gives:

```text
0 1 2 3 4 5 6 7 8 9
```

The second gives:

```text
10 11 12 13 14 15 16 17 18 19
```

The first range ends before 10, and the second range starts at 10. There is no overlap and no gap.

```text
0 ───────── 10 ───────── 20
│            │            │
range 1      range 2
```

That's one of the reasons this convention is useful.

## Timestamp Example from the Book

Imagine a full day:

```text
00:00:00 → 24:00:00
```

With closed-open thinking:

```text
[00:00:00, 24:00:00)
```

means: start included, end excluded. So you don't need to write something awkward like:

```text
00:00:00 → 23:59:59.999
```

The next day's starting point is simply:

```text
24:00:00
```

The exact timestamp details aren't something you need to memorize now. The important idea is the **boundary**.

## Connect This Back to Python

When you write `range(5)`, Python effectively gives you `0 → 5` with the ending boundary excluded:

```text
0  1  2  3  4  [5)
```

That's why:

```python
for i in range(5):
    print(i)
```

prints:

```text
0
1
2
3
4
```

## Remember

Python's `range()` follows a closed-open pattern: **include the start, exclude the end.**

And the practical benefit:

```text
range(start, stop)
number of values = stop - start
```

For example:

```text
range(3, 8)

8 - 3 = 5 values

3, 4, 5, 6, 7
```

This closed-open idea is worth understanding, but you don't need to overthink it. With practice, `range()` will become natural.

```text
════════════════════════ PART 2 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 3](#part-3)

---

<a id="part-3"></a>

```text
═══════════════════════ PART 3 : START ═══════════════════════
```

# Part 3: for vs while

The book's main point:

> A `for` loop can be written as an equivalent `while` loop, but the `for` loop is more concise.

## `for` Version

```python
print('Hello!')
for i in range(5):
    print('On this iteration, i is set to ' + str(i))
print('Goodbye!')
```

## Rewritten Using `while`

```python
print('Hello!')
i = 0
while i < 5:
    print('On this iteration, i is set to ' + str(i))
    i = i + 1
print('Goodbye!')
```

Both produce the same output.

## Let's See Why

### `for` version

```python
for i in range(5):
```

Python handles the iteration values for us:

```text
i = 0
i = 1
i = 2
i = 3
i = 4
```

### `while` version

We have to manage it ourselves:

```python
i = 0
```

Then:

```python
while i < 5:
```

and at the end:

```python
i = i + 1
```

So:

```text
i = 0
   ↓
0 < 5 → True
   ↓
run block
   ↓
i = 1
   ↓
1 < 5 → True
   ↓
run block
   ↓
i = 2
   ↓
...
```

Eventually:

```text
i = 5

5 < 5 → False
        ↓
      stop
```

## Compare the Amount of Work

### `for`

```python
for i in range(5):
    print(i)
```

Very concise.

### Equivalent `while`

```python
i = 0
while i < 5:
    print(i)
    i = i + 1
```

More things to manage:

```text
starting value
     ↓
condition
     ↓
increment
```

That's why the book says:

> `for` loops are useful for looping a specific number of times.
> `while` loops are useful for looping as long as a particular condition is true.

## The Easiest Way to Think About It

**FOR:** "Give me each value in this range."

```python
for i in range(5):
```

```text
0 → 1 → 2 → 3 → 4
```

**WHILE:** "Keep going while this condition is true."

```python
while i < 5:
```

```text
condition True  → continue
condition False → stop
```

## Important Connection

You now understand both ways of repeating code:

```text
                 LOOPS
                   │
          ┌────────┴────────┐
          ↓                 ↓
       while              for
          │                 │
   condition-based     iteration-based
          │                 │
   "while True..."     "for i in range..."
```

And you've already learned: `while`, `break`, `continue`, `for`, `range()`. That's a strong foundation for the next parts of Chapter 3.

```text
════════════════════════ PART 3 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 4](#part-4)

---

<a id="part-4"></a>

```text
═══════════════════════ PART 4 : START ═══════════════════════
```

# Part 4: Arguments to range()

Now `range()` becomes much more flexible. We already know `range(5)` means `0 1 2 3 4`. Now the book introduces multiple arguments.

## The Three Forms

`range()` can take 1, 2, or 3 arguments:

```text
range(stop)

range(start, stop)

range(start, stop, step)
```

## 1. `range(start, stop)`

Example from the book:

```python
for i in range(12, 16):
    print(i)
```

Here:

```text
start = 12
stop  = 16
```

Remember: start is included, stop is **NOT** included. So:

```text
12
13
14
15
```

Not 16.

### Mental model

```text
12 → 13 → 14 → 15 → [16]
 ↑                       ↑
start                  excluded
```

## 2. `range(start, stop, step)`

Now we add a third argument:

```python
for i in range(0, 10, 2):
    print(i)
```

Break it down:

```text
start = 0
stop  = 10
step  = 2
```

The step tells Python how much to move after each iteration:

```text
0
 ↓ +2
2
 ↓ +2
4
 ↓ +2
6
 ↓ +2
8
 ↓ +2
10 → STOP (excluded)
```

Output:

```text
0
2
4
6
8
```

### Think of step as the jump size

Without step:

```text
range(0, 5)

0 → 1 → 2 → 3 → 4
```

Step of 2:

```text
range(0, 10, 2)

0 → 2 → 4 → 6 → 8
```

Step of 3:

```text
range(0, 10, 3)

0 → 3 → 6 → 9
```

The important idea:

> Step controls how much the value changes between iterations.

## 3. Negative Step: Count Down

This is the interesting one. The book uses:

```python
for i in range(5, -1, -1):
    print(i)
```

Break it down:

```text
start = 5
stop  = -1
step  = -1
```

Because the step is negative:

```text
5
 ↓ -1
4
 ↓ -1
3
 ↓ -1
2
 ↓ -1
1
 ↓ -1
0
 ↓ -1
-1 → STOP
```

Because `-1` is the stop value and is excluded, the output is:

```text
5
4
3
2
1
0
```

### One important thing

Don't think: *"Negative step means negative numbers."* That's not the idea. It means:

> Move backward by that amount.

For example:

```text
range(10, 0, -2)

10 → 8 → 6 → 4 → 2
```

The values are positive, but we're counting downward.

## Complete `range()` Mental Model

```text
range(stop)
      ↓
start automatically = 0
step automatically = 1
```

Example:

```text
range(5)

0 1 2 3 4
```

```text
range(start, stop)
      ↓
step automatically = 1
```

Example:

```text
range(3, 7)

3 4 5 6
```

```text
range(start, stop, step)
```

Example:

```text
range(2, 10, 2)

2 4 6 8
```

And:

```text
range(5, -1, -1)

5 4 3 2 1 0
```

## Remember

```text
range(start, stop, step)
      │       │      │
      │       │      └── how much to move
      │       └───────── where to stop (excluded)
      └───────────────── where to start (included)
```

The stop value is always excluded.

## Your Turn

Don't run it. Trace this:

```python
for i in range(10, 2, -2):
    print(i)
```

Write only the output:

```text
?
?
?
?
```

And explain why it stops where it stops.

```text
════════════════════════ PART 4 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 5](#part-5)

---

<a id="part-5"></a>

```text
═══════════════════════ PART 5 : START ═══════════════════════
```

# Part 5: Importing Modules

Modules are how your Python programs start using useful functionality that you didn't write yourself. We'll stay with the book's explanation and build the mental model step by step.

## What Is a Module?

The book's basic idea:

> A module is a Python program containing a related group of functions that another Python program can use.

```text
Python Standard Library
│
├── random → random-number functions
├── math   → math functions
├── os     → operating-system functions
└── sys    → system-related functions
```

These are part of Python's standard library.

## 1. Built-in Functions vs Modules

You've already used functions such as:

```python
print()
input()
len()
```

These are **built-in functions**. You don't need to import anything before using them:

```python
print("Hello")
```

But `randint()` belongs to the `random` module. So first:

```python
import random
```

Then:

```python
random.randint(1, 10)
```

## 2. What Does `import` Actually Do?

When Python sees `import random`, you can mentally think:

```text
Your program
     │
     │ import random
     ↓
Make the random module available
     │
     ├── randint()
     └── other random functionality
```

Now your program can access things inside `random`.

## 3. `module.function()`

This is the key pattern:

```python
random.randint(1, 10)
```

Break it apart:

```text
random.randint(1, 10)
│      │
│      └── function
└── module
```

So `random.randint()` means:

> Use the `randint()` function that belongs to the `random` module.

The `random.` part tells Python where to look for `randint()`.

## 4. Trace the Book's Program

```python
import random

for i in range(5):
    print(random.randint(1, 10))
```

**Step 1**

```python
import random
```

Make the `random` module available.

**Step 2**

```python
for i in range(5):
```

`range(5)` gives `0 1 2 3 4`, so the loop runs 5 times.

**Step 3**

Each iteration executes:

```python
print(random.randint(1, 10))
```

Each time, `randint()` produces a random integer from 1 to 10. For example:

```text
Iteration 1 → 4
Iteration 2 → 1
Iteration 3 → 8
Iteration 4 → 4
Iteration 5 → 1
```

The next time you run the program, you can get completely different numbers.

## Your Turn: Experiment

Open your Python REPL and run:

```python
import random
```

Then:

```python
random.randint(1, 10)
```

Run it several times. Then try:

```python
random.randint(100, 200)
```

Question: what is the smallest number this can produce? And what is the largest?

The random result you see doesn't matter. The question is about the possible range.

## Important: Don't Name Your File `random.py`

This part of the book is extremely important. Suppose you create your own file:

```text
random.py
```

and inside another program you write:

```python
import random
```

Python may end up loading **your** `random.py` instead of the real `random` module. Then `random.randint(1, 10)` could fail because your file doesn't contain `randint()`. You could get something like:

```text
AttributeError:
module 'random' has no attribute 'randint'
```

### Simple rule

> Don't name your Python files after modules you're trying to import.

For example, avoid:

```text
random.py
math.py
os.py
sys.py
```

For your own projects, names like these are much safer:

```text
port_scanner.py
login_checker.py
recon.py
main.py
```

## Multiple Modules

The book also shows:

```python
import random, sys, os, math
```

This imports four modules:

```text
random
sys
os
math
```

Then you can access their functions using their module names. For example `random.randint(...)`, and later you'll learn things from `os...`, `math...` and `sys...`.

## `from random import *`

The book also shows another form:

```python
from random import *
```

With this form, you could use:

```python
randint(1, 10)
```

instead of:

```python
random.randint(1, 10)
```

because the `random.` prefix isn't required. But the book recommends:

```python
import random
```

because `random.randint(1, 10)` is clearer. You can immediately see:

```text
random  → module
randint → function
```

That's better for readability.

## The Whole Section in One Picture

```text
                 Python program
                       │
                       │ import random
                       ↓
                ┌──────────────┐
                │ random module│
                └──────┬───────┘
                       │
                       ↓
                  randint()
                       │
                       ↓
              random.randint(1,10)
                       │
                       ↓
                  random integer
```

## Remember

> `import module` makes the module available; `module.function()` uses a function inside that module.

### One important distinction

Don't memorize `random.randint()` as one mysterious command. Understand the structure:

```text
module.function()
```

You will see this pattern everywhere in Python, and later it becomes very important when we start building your cybersecurity tools.

## Quick Check

```python
random.randint(100, 200)
```

```text
Smallest possible = ?
```

```text
════════════════════════ PART 5 : END ════════════════════════
```

[Back to contents](#contents)

---

```text
═════════════ END OF FOR LOOPS AND MODULES NOTES ═════════════
```
