# Python Learning Notes: Day 2

Day 2 covers `print()`, `input()`, type conversion, built-in functions, binary, the Chapter 1 review, and the start of Chapter 2 (Booleans, comparison operators, Boolean operators).

<a id="contents"></a>

## Contents

1. [Part 1: print() and input()](#part-1)
2. [Part 2: Expressions, len() and Return Values](#part-2)
3. [Part 3: Type Conversion: str(), int() and float()](#part-3)
4. [Part 4: round() and abs()](#part-4)
5. [Part 5: Binary: How Computers Store Data](#part-5)
6. [Part 6: Chapter 1 Practice Questions and Answers](#part-6)
7. [Part 7: Chapter 1 One-Page Memory Sheet](#part-7)
8. [Part 8: Chapter 2: If-Else and Flow Control](#part-8)
9. [Part 9: Boolean Values](#part-9)
10. [Part 10: Comparison Operators](#part-10)
11. [Part 11: Boolean Operators: and, or, not](#part-11)
12. [Part 12: Combining Comparison and Boolean Operators](#part-12)

---

<a id="part-1"></a>

```text
═══════════════════════ PART 1 : START ═══════════════════════
```

# Part 1: print() and input()

Today we move from **variables** into two very important functions:

* `print()` → show information to the user
* `input()` → receive information from the user

## 1. `print()` Function

You already used this yesterday:

```python
print("Got A Job")
```

The job of `print()` is:

> **Display something on the screen.**

For example:

```python
print('Hello, world!')
```

Output:

```text
Hello, world!
```

### What is happening?

```text
print("Hello")
      ↑
   argument
```

The string `"Hello"` is being **passed to the `print()` function**.

The value passed to a function is called an **argument**.

So:

```python
print("Hello")
```

means roughly:

```text
Call print()
     ↓
Give it "Hello"
     ↓
Display Hello
```

## 2. Why don't we see the quotes?

When you write:

```python
print('Hello')
```

Python doesn't print:

```text
'Hello'
```

It prints:

```text
Hello
```

Because the quotes tell Python:

> **This is a string.**

The quotes aren't part of the actual string's text.

## 3. `print()` can print a blank line

You can simply do:

```python
print()
```

Nothing is displayed, but Python moves to the next line.

For example:

```python
print("Hello")
print()
print("World")
```

Output:

```text
Hello

World
```

## 4. `print` vs `print()`

This is important:

```python
print()
```

is a **function call**.

The `()` tells Python:

> We're calling the function.

For now, remember:

```text
print    → function name
print()  → call the function
```

## 5. `input()` Function

Now we have the opposite direction.

`print()`:

```text
Program → User
```

`input()`:

```text
User → Program
```

Example:

```python
my_name = input('>')
```

When the program reaches this line, it waits for you to type something.

You might see:

```text
>
```

You type:

```text
Harish
```

and press Enter.

Python receives:

```text
"Harish"
```

and stores it:

```text
my_name → "Harish"
```

## Very important: `input()` gives you a string

Suppose you type:

```text
25
```

You might think:

```text
25 → integer
```

But `input()` gives your program **text**.

So:

```python
age = input('>')
```

and you type:

```text
25
```

means:

```text
age → "25"
```

not:

```text
age → 25
```

That difference will become very important when we learn `str()`, `int()`, and `float()`.

## `>` vs `>>>`

The book specifically wants you to understand this.

### `>>>`

```text
>>>
```

means you're interacting directly with the **Python REPL**.

Example:

```text
>>> 2 + 2
4
```

### `>`

If your program contains:

```python
input('>')
```

then the program can display:

```text
>
```

and wait for the user.

So:

```text
>>> → Python interactive shell prompt

>   → prompt chosen by the programmer for input()
```

They are **not the same thing**.

## Your Day 2 practice

Open your Python REPL.

First:

```python
print("Hello, Harish")
```

Then:

```python
print()
```

Then:

```python
name = input('>')
```

Type your name when it asks.

Then:

```python
name
```

You should see the name you entered.

For example:

```text
> Harish
>>> name
'Harish'
```

Now try this:

```python
name = input("What is your name? ")
```

You'll get something like:

```text
What is your name? Harish
```

Then:

```python
print(name)
```

Output:

```text
Harish
```

## The complete flow

This is the important mental model for today:

```text
             USER
              │
              │ types information
              ▼
          input()
              │
              ▼
          VARIABLE
              │
              ▼
          print()
              │
              ▼
             USER
```

Example:

```python
name = input("What is your name? ")
print(name)
```

```text
Program
   │
   ├── asks → What is your name?
   │
   ├── user types → Harish
   │
   ├── stores → name = "Harish"
   │
   └── prints → Harish
```

### Remember

> **`print()` sends information from the program to the screen. `input()` gets text from the user and returns it as a string.**

```text
════════════════════════ PART 1 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 2](#part-2)

---

<a id="part-2"></a>

```text
═══════════════════════ PART 2 : START ═══════════════════════
```

# Part 2: Expressions, len() and Return Values

This part connects three ideas: **expressions → functions → return values**.

## 1. Greeting message

The book gives:

```python
print('It is good to meet you, ' + my_name)
```

Suppose:

```python
my_name = 'Al'
```

First Python evaluates the expression **inside** `print()`:

```text
'It is good to meet you, ' + 'Al'
                    ↓
'It is good to meet you, Al'
```

Then that resulting string is passed to `print()`:

```text
expression
    ↓
"It is good to meet you, Al"
    ↓
print()
    ↓
screen
```

So remember:

> **Python evaluates the expression first, then `print()` displays the resulting value.**

## 2. `len()` function

Now we get another function:

```python
len()
```

`len()` means:

> **Find how many characters are in a string.**

Example:

```python
len('hello')
```

Result:

```text
5
```

Because:

```text
h e l l o
1 2 3 4 5
```

Another:

```python
len('')
```

Result:

```text
0
```

Because the empty string contains **zero characters**.

## `len()` RETURNS a value

This is an important programming concept.

When you write:

```python
len('hello')
```

Python doesn't just "do something."

It **produces a value**:

```text
len('hello')
      ↓
      5
```

That `5` is called the **return value** of `len()`.

And because `5` is an integer, you can use it in another expression.

For example:

```python
len('hello') + 10
```

Python:

```text
len('hello')
     ↓
     5

5 + 10
  ↓
15
```

That's a powerful idea:

> **A function call can itself be part of an expression because it can produce a value.**

## 3. `print()` can receive different types

For example:

```python
print(29)
```

works.

```python
print('Hello')
```

also works.

So `print()` can display both:

```text
int
str
```

But look at this:

```python
print('I am ' + 29 + ' years old.')
```

 Error.

## Why is it NOT `print()`'s fault?

This is an important point from the book.

Python has to evaluate this first:

```python
'I am ' + 29 + ' years old.'
```

But it sees:

```text
str + int
```

and we already learned:

```text
str + str → concatenate ✓
int + int → add ✓
str + int → ✗
```

So Python fails **before `print()` can even receive the result**.

Think:

```text
print(
    'I am ' + 29 + ' years old.'
          ↓
      ERROR HERE ✗
)
```

`print()` never gets a valid value.

## Try this in your REPL

First:

```python
my_name = 'Harish'
```

Then:

```python
print('It is good to meet you, ' + my_name)
```

Then:

```python
len('hello')
```

Then:

```python
len(my_name)
```

Then:

```python
len('hello') + 10
```

Finally, intentionally create the error:

```python
'I am ' + 29 + ' years old.'
```

Look carefully at the error.

## Today's important mental model

You now have:

```text
Expression
    ↓
Evaluation
    ↓
Single value
```

And functions can participate in that:

```text
len('hello')
     ↓
   5
     ↓
return value
```

Then that returned value can be used elsewhere:

```python
len('hello') + 10
```

```text
len('hello')
     ↓
     5
     ↓
  5 + 10
     ↓
    15
```

### Remember

> **`len()` returns the number of characters in a string, and a function's return value can be used as part of another expression.**

This is a **very important foundation**. Once this clicks, functions will become much easier later.

```text
════════════════════════ PART 2 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 3](#part-3)

---

<a id="part-3"></a>

```text
═══════════════════════ PART 3 : START ═══════════════════════
```

# Part 3: Type Conversion: str(), int() and float()

You first tried:

```python
print("Your Name contain tottaly: " + len(name) + "Character's")
```

and Python said:

```text
TypeError: can only concatenate str (not "int") to str
```

Then you tried:

```python
int(len(name))
```

and got the **same error**.

Then you used:

```python
str(len(name))
```

and it worked.

Let's understand **why**.

## 1. `str()` = convert to string

Your:

```python
len(name)
```

returns:

```text
6
```

But `6` is an **integer**.

So you had:

```text
"Your Name contain tottaly: "
          +
        6
          +
" Character's"
```

That's:

```text
str + int + str
```

 Python doesn't allow that concatenation.

When you do:

```python
str(len(name))
```

Python does:

```text
len(name)
   ↓
6          ← int
   ↓
str(6)
   ↓
"6"        ← str
```

Now you have:

```text
str + str + str
```

That's why this works:

```python
print("Your Name contain tottaly: " + str(len(name)) + " Character's")
```

## Why didn't `int(len(name))` work?

This is an excellent thing you discovered.

You wrote:

```python
int(len(name))
```

But `len(name)` was **already an integer**.

```text
len(name)
   ↓
6
   ↓
int(6)
   ↓
6
```

You still have:

```text
int
```

So your expression remained:

```text
str + int + str
```

and failed.

### Simple rule

```text
str()  → make it text
int()  → make it an integer
float() → make it a float
```

You used `int()` when you needed `str()`.

## 2. `int()` — convert to integer

The book gives:

```python
int('42')
```

The original value:

```text
'42'
```

is a **string**.

After:

```python
int('42')
```

you get:

```text
42
```

which is an **integer**.

This is especially useful with `input()`.

Remember:

```python
age = input('>')
```

If you type:

```text
25
```

Python stores:

```text
age → '25'
```

not:

```text
age → 25
```

So:

```python
int(age)
```

converts it:

```text
'25'
 ↓
25
```

Now you can do mathematics:

```python
int(age) + 1
```

→ `26`

## 3. `float()` — convert to floating-point

Example:

```python
float('3.14')
```

gives:

```text
3.14
```

And:

```python
float(10)
```

gives:

```text
10.0
```

So:

```text
int → whole number
float → decimal number
str → text
```

## Now understand the book's age example

The code is:

```python
my_age = input('>')
print('You will be ' + str(int(my_age) + 1) + ' in a year.')
```

Suppose you enter:

```text
4
```

### Step 1 — `input()`

```text
my_age = '4'
```

It's a **string**.

### Step 2 — `int(my_age)`

```text
int('4')
   ↓
4
```

Now it's an integer.

### Step 3 — add 1

```text
4 + 1
 ↓
5
```

### Step 4 — convert back to string

We need text because we're joining it with other strings:

```text
str(5)
 ↓
'5'
```

### Step 5 — concatenate

```text
'You will be ' + '5' + ' in a year.'
```

becomes:

```text
'You will be 5 in a year.'
```

### Step 6 — print

```text
You will be 5 in a year.
```

## The complete flow

This diagram is worth remembering:

```text
input()
   ↓
'4'          ← string
   ↓
int()
   ↓
4            ← integer
   ↓
+ 1
   ↓
5            ← integer
   ↓
str()
   ↓
'5'          ← string
   ↓
concatenation
   ↓
'You will be 5 in a year.'
   ↓
print()
```

Notice something interesting:

**We convert from string → integer → string.**

Why?

Because we need to perform **math in the middle**, then combine the result with text.

## `int()` cannot convert everything

This works:

```python
int('99')
```

→ `99`

But:

```python
int('99.99')
```

And:

```python
int('twelve')
```

Because those strings don't represent valid integers in the way `int()` expects.

## One more important point: `42` vs `'42'`

The book shows:

```python
42 == '42'
```

→ `False`

Because:

```text
42   → int
'42' → str
```

But:

```python
42 == 42.0
```

→ `True`

because they are different numeric types representing the same numeric value:

```text
42    → int
42.0  → float
```

## Your experiment taught you something important

You didn't just read:

> "`str()` converts an integer to a string."

You actually discovered:

```text
len(name)
   ↓
int

int(len(name))
   ↓
still int ✗

str(len(name))
   ↓
string ✓
```

### Remember these three

```text
str(42)      → '42'
int('42')    → 42
float('3.14') → 3.14
```

And the most important rule from today's lesson:

> **Use `int()` when you need to do mathematics with a numeric string. Use `str()` when you need to combine a number with text.**

```text
════════════════════════ PART 3 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 4](#part-4)

---

<a id="part-4"></a>

```text
═══════════════════════ PART 4 : START ═══════════════════════
```

# Part 4: round() and abs()

This part covers the last small group of built-in functions: `round()` and `abs()`.

The main lesson is actually bigger than these two functions:

> **A function can receive a value, process it, and return a new value.**

## 1. `round()` — round a number

`round()` takes a number and returns a rounded value.

```python
round(3.14)
```

→

```text
3
round(7.7)
```

→

```text
8
round(-2.2)
```

→

```text
-2
```

Think:

```text
round(3.14)
      ↓
    3
```

### Choosing decimal places

You can give `round()` a second argument.

```python
round(3.14, 1)
```

→

```text
3.1
```

The `1` means:

> Round to **1 decimal place**.

Another:

```python
round(7.7777, 3)
```

→

```text
7.778
```

Here:

```text
7.7777
    ↓
3 decimal places
    ↓
7.778
```

So:

```python
round(number, decimal_places)
```

## One interesting case: `.5`

The book points out something that surprises beginners:

```python
round(3.5)
```

→ `4`

But:

```python
round(2.5)
```

→ `2`

Why?

Python uses **banker's rounding** for halfway cases: when the value is exactly halfway between two integers, it chooses the **nearest even integer**.

So:

```text
3.5 → 4   (4 is even)
2.5 → 2   (2 is even)
```

You don't need to memorize lots of special cases right now. Just know that **`.5` doesn't always simply mean "round upward" in Python.**

## 2. `abs()` — absolute value

`abs()` gives the **absolute value** of a number.

Simple mental model:

> **How far is this number from zero?**

For example:

```python
abs(25)
```

→ `25`

Because 25 is 25 units from zero.

```python
abs(-25)
```

→ `25`

Because -25 is also 25 units from zero.

So:

```text
-25 ───────── 0 ───────── 25
  ↑            ↑           ↑
25 units     zero       25 units
```

Both `25` and `-25` have an absolute value of `25`.

Also:

```python
abs(-3.14)
```

→ `3.14`

and:

```python
abs(0)
```

→ `0`

## Connect this to what we've learned

You now know several built-in functions:

```text
print()  → displays a value
input()  → gets text from the user
len()    → returns number of characters
str()    → converts to string
int()    → converts to integer
float()  → converts to float
round()  → rounds a number
abs()    → returns absolute value
```

And they all follow the same basic pattern:

```text
function(value)
       ↓
   processing
       ↓
 return value
```

For example:

```python
len("Harish")
```

```text
"Harish"
   ↓
len()
   ↓
6
```

And:

```python
abs(-25)
```

```text
-25
 ↓
abs()
 ↓
25
```

## Your practice

Run these yourself:

```python
round(3.14)
round(7.7777, 2)
abs(-100)
```

Then experiment with your own values.

For example:

```python
round(123.456, 1)
```

and:

```python
abs(-42)
```

### Remember

> **`round()` changes a number to a rounded value; `abs()` gives the number's nonnegative distance from zero.**

And the bigger Chapter 1 lesson is becoming clear:

**Python expressions are built by combining values, operators, variables, and function calls—and those expressions evaluate to values.**

```text
════════════════════════ PART 4 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 5](#part-5)

---

<a id="part-5"></a>

```text
═══════════════════════ PART 5 : START ═══════════════════════
```

# Part 5: Binary: How Computers Store Data

This part is a short computer-science foundation, not really Python coding. The goal is to understand what is happening underneath the values Python works with.

## 1. What is binary?

Our normal number system is **decimal (base 10)**:

```text
0 1 2 3 4 5 6 7 8 9
```

Binary is **base 2**, so it only has:

```text
0 1
```

That's it.

The important thing is:

> **Binary `10` does NOT mean decimal ten. It means decimal 2.**

For example:

```text
Decimal    Binary
   0          0
   1          1
   2         10
   3         11
   4        100
   5        101
   6        110
   7        111
   8       1000
```

The reason is similar to a decimal odometer.

Decimal:

```text
... 8
... 9
... 10
```

Binary:

```text
... 0
... 1
... 10
... 11
... 100
```

When binary reaches `1`, the next position carries over.

## 2. Why do computers use binary?

The book's main idea is **two physical states are easier to represent reliably**.

For example:

```text
Electricity
ON  → 1
OFF → 0
```

Hardware can work with two states very reliably.

Trying to build hardware that reliably distinguishes ten different electrical levels is more complicated.

So:

```text
Binary
  ↓
0 / 1
  ↓
simple physical states
  ↓
computer hardware
```

Don't think that the computer has tiny people inside typing `01010101`.

The `0` and `1` are a human-friendly way of representing underlying states/information.

## 3. Bits and Bytes

A **bit** is one binary digit:

```text
0
```

or:

```text
1
```

So:

```text
1 bit → 2 possible values
```

With 2 bits:

```text
00
01
10
11
```

That's 4 possibilities.

With 8 bits:

```text
00000000
```

through:

```text
11111111
```

That's **256 possible combinations**.

Therefore:

```text
8 bits = 1 byte
```

and an unsigned byte can represent:

```text
0 → 255
```

This is something you'll encounter constantly in cybersecurity.

For example, IPv4 addresses use **32 bits**:

```text
8 bits . 8 bits . 8 bits . 8 bits
```

which is why each IPv4 octet normally ranges from:

```text
0 → 255
```

That connects directly to the networking you've already learned.

## 4. Binary and Python

You might wonder:

> "If computers internally use binary, why can I type `42` in Python?"

Because Python gives you a convenient high-level interface.

You write:

```python
42
```

Python understands it as an integer.

Underneath, computers ultimately represent information using binary.

You don't need to manually convert everything to binary when programming.

## 5. Binary can represent much more than numbers

This is the **biggest idea in this section**.

Binary isn't just for storing numbers.

Computers need to represent:

```text
Numbers
   ↓
Text
   ↓
Images
   ↓
Audio
   ↓
Video
   ↓
Programs
   ↓
Everything digital
```

The general idea is:

> **Information is encoded into numbers, and those numbers can be represented in binary.**

## Text example: `"Hello"`

The book explains that text uses an **encoding**.

UTF-8 is the dominant text encoding today.

For example:

```text
H → 72
e → 101
l → 108
l → 108
o → 111
```

Those numbers can then be represented in binary.

So conceptually:

```text
"Hello"
   ↓
characters
   ↓
encoded numbers
   ↓
binary
   ↓
bits
```

That's why the book can show:

```text
01001000...
```

and say that it represents text.

## Images

Images can also be represented numerically.

A digital image consists of **pixels**.

A simple RGB pixel can use three components:

```text
Red
Green
Blue
```

For example:

```text
255, 0, 255
```

represents maximum red + maximum blue + no green, producing purple.

Again:

```text
Image
 ↓
pixels
 ↓
numbers
 ↓
binary
```

## Audio and Video

Same basic idea.

### Audio

Sound can be sampled and represented using numbers.

```text
Sound
 ↓
measurements
 ↓
numbers
 ↓
binary
```

### Video

A video combines image data and audio data.

```text
Video
 ├── image data
 └── audio data
        ↓
      numbers
        ↓
      binary
```

## Storage units

The chapter also introduces:

```text
1 byte = 8 bits
1 KB ≈ 1,024 bytes
1 MB ≈ 1,024 KB
1 GB ≈ 1,024 MB
1 TB ≈ 1,024 GB
```

The book is using the traditional binary-based definitions here.

You may also encounter **KiB, MiB, GiB, TiB**, which are the standardized names for the 1024-based units. You don't need to dive into that distinction for this chapter.

## Why this matters for YOUR cybersecurity path

This isn't something you need to memorize heavily for Python.

But you've already encountered the underlying idea in networking.

For example:

```text
IPv4
↓
32 bits
↓
4 × 8-bit sections
↓
192.168.1.10
```

And in Wireshark you were looking at:

```text
Packet
 ↓
Bytes
 ↓
Hexadecimal
 ↓
Binary underneath
 ↓
Protocol fields
```

So this chapter is connecting something you've already been seeing:

```text
Networking / Wireshark
        ↓
     Bytes
        ↓
   Binary information
        ↓
    Computer data
```

## Don't over-study this section

Remember only these:

```text
Binary = base 2 → 0 and 1

Bit = one binary digit

8 bits = 1 byte

Computers represent different kinds of information
using encoded numerical data.

Text → encoding → numbers → binary
Images → pixels → numbers → binary
Audio → measurements → numbers → binary
```

You **do not need to memorize the entire decimal/binary table**.

You don't need to stop Python learning to become a binary expert. This part only gives you the mental foundation.

```text
════════════════════════ PART 5 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 6](#part-6)

---

<a id="part-6"></a>

```text
═══════════════════════ PART 6 : START ═══════════════════════
```

# Part 6: Chapter 1 Practice Questions and Answers

## 1. Which are operators and which are values?

Given:

```text
*
'hello'
-88.8
-
/
+
5
```

### Answer

**Operators:**

```text
*
-
/
+
```

**Values:**

```text
'hello'
-88.8
5
```

### Remember

> Operators perform an operation; values are the data the operation works with.

## 2. Which is a variable and which is a string?

```text
spam
'spam'
```

### Answer

```text
spam     → variable
'spam'   → string
```

The quotes are important.

```python
spam
```

means Python looks for a variable named `spam`.

```python
'spam'
```

means the actual text `"spam"`.

### Remember

> **Quotes → string. No quotes → potentially a variable/name.**

## 3. Name three data types.

### Answer

```text
int   → integer
float → floating-point number
str   → string
```

Examples:

```python
42       # int
3.14     # float
'hello'  # str
```

## 4. What is an expression made up of? What do all expressions do?

### Answer

Expressions can contain:

* Values
* Operators
* Variables
* Function calls

Example:

```python
spam + 10
```

An expression **evaluates to a single value**.

Example:

```python
2 + 3
```

evaluates to:

```text
5
```

### Remember

> **Expression → evaluation → one resulting value.**

## 5. What is the difference between an expression and a statement?

### Answer

An **expression** evaluates to a value.

```python
2 + 3
```

→ `5`

A **statement** is an instruction that performs an action.

```python
spam = 10
```

→ assigns `10` to `spam`.

### Remember

```text
Expression → produces a value
Statement   → performs an instruction/action
```

## 6. What does `bacon` contain?

```python
bacon = 20
bacon + 1
```

### Answer

`bacon` still contains:

```text
20
```

Why?

First:

```python
bacon = 20
```

stores `20`.

Then:

```python
bacon + 1
```

calculates:

```text
20 + 1 = 21
```

But **the result is not assigned back to `bacon`**.

So:

```text
bacon → 20
```

If the code were:

```python
bacon = bacon + 1
```

then `bacon` would contain:

```text
21
```

### Important

> **Calculating a new value doesn't change a variable unless you assign the result back to it.**

## 7. What do these expressions evaluate to?

### A

```python
'spam' + 'spamspam'
```

`+` joins strings:

```text
'spam' + 'spamspam'
       ↓
'spamspamspam'
```

#### Answer

```text
'spamspamspam'
```

### B

```python
'spam' * 3
```

`*` with a string and an integer repeats the string:

```text
spam
spam
spam
```

#### Answer

```text
'spamspamspam'
```

### Remember

```text
str + str → concatenation
str * int → replication
```

## 8. Why is `eggs` valid but `100` invalid as a variable name?

### Answer

`eggs` is a valid variable name:

```python
eggs = 10
```

But:

```python
100 = 10
```

is invalid.

The reason is that **`100` is a number/value, not a valid variable name**.

Python variable names follow naming rules and cannot start with a number.

For example:

```text
eggs       ✓
my_age     ✓
age2       ✓

100        ✗
2eggs      ✗
```

### Remember

> **A variable name can use letters, numbers, and underscores, but it cannot start with a number.**

## 9. What three functions convert values to int, float, or string?

### Answer

```python
int()
float()
str()
```

Their jobs:

```text
int()   → integer
float() → floating-point number
str()   → string
```

Examples:

```python
int('42')       # 42
float('3.14')   # 3.14
str(42)         # '42'
```

### Remember

> **`int()` → whole number, `float()` → decimal number, `str()` → text.**

## 10. Why does this cause an error, and how do you fix it?

```python
'I eat ' + 99 + ' burritos.'
```

### Why does it fail?

Python sees:

```text
str + int + str
```

But `+` can:

```text
number + number      → addition
string + string      → concatenation
```

It cannot directly concatenate:

```text
string + integer
```

So Python gives a `TypeError`.

### Fix

Convert `99` into a string:

```python
'I eat ' + str(99) + ' burritos.'
```

Result:

```text
'I eat 99 burritos.'
```

### Remember

> **When joining a number with strings using `+`, convert the number to a string with `str()`.**

```text
════════════════════════ PART 6 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 7](#part-7)

---

<a id="part-7"></a>

```text
═══════════════════════ PART 7 : START ═══════════════════════
```

# Part 7: Chapter 1 One-Page Memory Sheet

```text
VALUES
├── int       → 42
├── float     → 3.14
└── str       → 'hello'

OPERATORS
├── +         → addition / string concatenation
├── -         → subtraction
├── *         → multiplication / string replication
├── /         → division
├── //        → integer division
├── %         → remainder
└── **        → exponentiation

VARIABLE
name = value

Example:
age = 25

EXPRESSION
Something Python evaluates → produces a value

Example:
2 + 3 → 5

STATEMENT
An instruction that performs an action

Example:
age = 25

FUNCTIONS
print()  → display
input()  → get user text
len()    → number of characters
str()    → convert to string
int()    → convert to integer
float()  → convert to float
round()  → round number
abs()    → absolute value
```

## 5 things to remember from Chapter 1

1. **Expression → evaluates to a value.**
2. **Statement → performs an instruction/action.**
3. **Variable → name associated with a value.**
4. **Data type matters: `42` ≠ `'42'`.**
5. **Functions can take arguments and return values.**

That's the real foundation of Chapter 1. You don't need to memorize the 10 answers word-for-word. If you understand the concepts above, you can **derive the answers yourself**, which is much more valuable for your Python learning.

More reading: <https://docs.python.org/3/builtins/functions.html>

```text
════════════════════════ PART 7 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 8](#part-8)

---

<a id="part-8"></a>

```text
═══════════════════════ PART 8 : START ═══════════════════════
```

# Part 8: Chapter 2: If-Else and Flow Control

Chapter 1 taught us how Python **works with values**.

Now Chapter 2 asks:

> **What if Python needs to make a decision?**

For example:

```text
Is it raining?
     │
   YES ──→ Do I have an umbrella?
     │              │
     │           YES│NO
     │              │
     │              ↓
     │          Wait a while
     │
    NO
     │
     ↓
Go outside
```

That is **flow control**.

## 1. What is "flow"?

Imagine Python reading your program:

```python
print("Step 1")
print("Step 2")
print("Step 3")
print("Step 4")
```

Normally:

```text
Step 1
  ↓
Step 2
  ↓
Step 3
  ↓
Step 4
```

Python goes from top → bottom.

But real programs often need to **choose a path**.

For example:

```text
          Is raining?
          /        \
       YES          NO
        ↓            ↓
   Take umbrella   Go outside
```

Now Python isn't simply following one straight line.

That's why we call it:

> **Flow control** — controlling which instructions Python executes and in what order.

## 2. Flowchart → Python

The book's flowchart uses different shapes.

### Rounded rectangle

```text
(Start)
```

Beginning/end.

### Rectangle

```text
┌──────────────┐
│ Go outside   │
└──────────────┘
```

An action.

### Diamond

```text
     /\
    /  \
   / ?  \
   \    /
    \  /
     \/
```

A **decision**.

For example:

```text
Is raining?
```

The answer can be:

```text
YES
NO
```

And each answer takes a different path.

## 3. But how does Python represent YES/NO?

This is exactly where the chapter is going next.

Python has a special data type called:

```python
bool
```

It has only two values:

```python
True
False
```

Think:

```text
YES → True
NO  → False
```

So:

```python
is_raining = True
```

means:

> Yes, it is raining.

And:

```python
is_raining = False
```

means:

> No, it isn't raining.

## 4. This connects directly to your `abs()` lesson

Remember:

```python
abs(-6)
```

→ `6`

That was a function that **returned a value**.

Now imagine:

```python
5 > 3
```

Python can evaluate that too.

The result isn't `8` or `"hello"`.

It is:

```python
True
```

And:

```python
2 > 10
```

becomes:

```python
False
```

So we're extending our Chapter 1 mental model:

```text
Expression
    ↓
Evaluation
    ↓
A value
```

The value can now be:

```text
42
3.14
"hello"
True
False
```

That's the bridge from **Chapter 1 → Chapter 2**.

## Why this matters

Eventually we'll be able to write:

```python
if age >= 18:
    print("You can continue.")
```

Python first evaluates:

```python
age >= 18
```

That produces:

```text
True
```

or:

```text
False
```

Then the `if` statement uses that result to decide what to execute.

So the fundamental flow becomes:

```text
Expression
    ↓
True / False
    ↓
Decision
    ↓
Choose which code runs
```

That's the **core idea of Chapter 2**.

## One tiny experiment before continuing

Open your Python REPL and try:

```python
5 > 3
```

Then:

```python
5 < 3
```

Then:

```python
10 == 10
```

Then:

```python
10 == 5
```

Don't worry about `if` yet.

Just observe:

```text
5 > 3   → ?
5 < 3   → ?
10 == 10 → ?
10 == 5  → ?
```

This is the foundation for the next section: **Boolean values and comparison operators**.

```text
════════════════════════ PART 8 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 9](#part-9)

---

<a id="part-9"></a>

```text
═══════════════════════ PART 9 : START ═══════════════════════
```

# Part 9: Boolean Values

This is the **first real building block of decision-making** in Chapter 2.

A **Boolean** is a data type that has only **two possible values**:

```python
True
False
```

Think of it simply as:

```text
True  → YES
False → NO
```

Unlike strings, Boolean values **do not have quotes**.

```python
True       # Boolean
False      # Boolean

'True'     # String
'False'    # String
```

That's an important difference.

## 1. Boolean can be stored in a variable

The book shows:

```python
spam = True
```

Now:

```python
spam
```

produces:

```text
True
```

So just like Chapter 1:

```python
age = 25
name = 'Harish'
spam = True
```

A variable can store different types of values.

```text
age  → 25       int
name → 'Harish' str
spam → True     bool
```

## 2. `True` and `true` are NOT the same

This is a very important Python syntax rule.

Correct:

```python
True
```

Incorrect:

```python
true
```

If you type:

```python
>>> true
```

Python gives:

```text
NameError: name 'true' is not defined
```

Why?

Because Python recognizes the Boolean value only with the exact capitalization:

```text
T + rue
F + alse
```

Not:

```text
true
false
TRUE
FALSE
```

For now, remember:

> **Python Boolean values are exactly `True` and `False`.**

## 3. `True` and `False` cannot be variable names

The book shows:

```python
False = 2 + 2
```

Python rejects it:

```text
SyntaxError: can't assign to False
```

Because `True` and `False` are special Python Boolean values.

You can't do:

```python
True = 10
```

or:

```python
False = 20
```

Think of them as **reserved special values**.

## The important connection

You already learned in Chapter 1:

```python
2 + 3
```

evaluates to:

```text
5
```

Now in Chapter 2, expressions can also evaluate to Boolean values.

For example:

```python
5 > 3
```

evaluates to:

```text
True
```

And:

```python
5 < 3
```

evaluates to:

```text
False
```

So:

```text
Expression
    ↓
Evaluation
    ↓
True / False
    ↓
Decision
```

That's the bridge between **expressions** and **flow control**.

## Try this in your REPL

Don't just read it. Run each one:

```python
>>> spam = True
>>> spam
```

Then:

```python
>>> type(spam)
```

Then:

```python
>>> 'True'
```

Compare that with:

```python
>>> True
```

You'll see that one is text and the other is a Boolean value.

Then try:

```python
>>> true
```

and observe the error.

Finally:

```python
>>> 5 > 3
>>> 5 < 3
```

## Your Chapter 2 mental model so far

```text
BOOLEAN
   │
   ├── True
   └── False
```

And:

```text
COMPARISON EXPRESSION
        ↓
    evaluates
        ↓
   True / False
        ↓
   flow control
```

### Remember

> **Boolean = a value that can only be `True` or `False`.**

Next, the book will introduce **comparison operators** (`==`, `!=`, `<`, `>`, `<=`, `>=`). Those are what let Python **produce these True/False values from conditions**.

```text
════════════════════════ PART 9 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 10](#part-10)

---

<a id="part-10"></a>

```text
══════════════════════ PART 10 : START ═══════════════════════
```

# Part 10: Comparison Operators

Comparison operators are one of the **most important parts of Chapter 2**.

Don't try to memorize the table first. Understand the idea.

## 1. What is a comparison operator?

A comparison operator **compares two values** and gives you only:

```text
True
```

or

```text
False
```

So:

```text
Two values
   ↓
Comparison
   ↓
True / False
```

For example:

```python
5 > 3
```

Python asks:

> Is 5 greater than 3?

Yes:

```text
True
```

## 2. The six comparison operators

| Operator | Meaning               | Example  | Result |
| -------- | --------------------- | -------- | ------ |
| `==`     | equal to              | `5 == 5` | `True` |
| `!=`     | not equal to          | `5 != 3` | `True` |
| `<`      | less than             | `3 < 5`  | `True` |
| `>`      | greater than          | `5 > 3`  | `True` |
| `<=`     | less than or equal    | `5 <= 5` | `True` |
| `>=`     | greater than or equal | `5 >= 3` | `True` |

The easiest way to learn them is to **ask the question in English**.

### `==` → "Are they equal?"

```python
42 == 42
```

Ask:

> Is 42 equal to 42?

```text
True
```

But:

```python
42 == 99
```

Ask:

> Is 42 equal to 99?

```text
False
```

### `!=` → "Are they different?"

```python
2 != 3
```

Ask:

> Is 2 different from 3?

```text
True
```

But:

```python
2 != 2
```

Ask:

> Is 2 different from 2?

```text
False
```

### `<` → "Is the left side smaller?"

```python
3 < 5
```

Ask:

> Is 3 less than 5?

```text
True
10 < 5
```

→ `False`

### `>` → "Is the left side bigger?"

```python
10 > 5
```

→ `True`

```python
2 > 8
```

→ `False`

### `<=` → "Smaller OR equal?"

This one is important.

```python
4 <= 5
```

→ `True`

Because 4 is smaller.

But:

```python
5 <= 5
```

→ `True`

Because they are equal.

So:

```text
<=
│
├── less than
└── OR equal
```

### `>=` → "Bigger OR equal?"

```python
5 >= 4
```

→ `True`

And:

```python
5 >= 5
```

→ `True`

Because equality is also allowed.

## The BIG confusion: `=` vs `==`

You already learned this in Chapter 1, but now it becomes **very important**.

### `=`

Assignment.

```python
age = 25
```

Means:

> Put the value `25` into the variable `age`.

It **changes/stores a value**.

### `==`

Comparison.

```python
age == 25
```

Means:

> Is the value inside `age` equal to 25?

It **asks a question**.

The result is:

```text
True
```

or:

```text
False
```

### Remember this forever

```text
=   → PUT
==  → ASK
```

Example:

```python
age = 25
```

```text
age ← 25
```

Then:

```python
age == 25
```

```text
Is age equal to 25?
       ↓
     True
```

## Let's connect this to your previous variable knowledge

Suppose:

```python
age = 25
```

Now try:

```python
age == 25
```

Python:

```text
25 == 25
   ↓
 True
```

Now:

```python
age > 18
```

becomes:

```text
25 > 18
  ↓
True
```

And:

```python
age < 18
```

becomes:

```text
25 < 18
  ↓
False
```

This is exactly how a real program can make decisions.

## Data types still matter

You learned this in Chapter 1:

```python
42 == 42.0
```

→ `True`

because both represent the same numeric value.

But:

```python
42 == '42'
```

→ `False`

because:

```text
42    → int
'42'  → str
```

The text `"42"` is not the same value/type as the integer `42`.

## Now see the complete flow

This is the important connection between Chapter 1 and Chapter 2:

```text
age = 25
   │
   ▼
age > 18
   │
   ▼
25 > 18
   │
   ▼
True
```

Then **flow control** can use that `True`.

Eventually:

```python
if age > 18:
    print("Allowed")
```

Python will essentially reason:

```text
age > 18
   ↓
25 > 18
   ↓
True
   ↓
execute print()
```

And if it were `False`, Python would take another path.

**That's why Boolean values + comparison operators come before `if`.**

## Your turn — don't look for the answers

Run these in your REPL:

```python
10 == 10
10 == 20
10 != 20
10 != 10
5 < 10
10 < 5
10 > 5
5 > 10
5 <= 5
5 >= 5
```

Then try variables:

```python
age = 25

age == 25
age > 18
age < 18
age >= 25
age <= 20
```

For each one, **say the English question in your head first**, then look at Python's `True`/`False`.

### Core memory

> **Comparison operators ask a question about two values. The answer is always `True` or `False`.**

And the six are:

```text
==   equal
!=   not equal
<    less
>    greater
<=   less/equal
>=   greater/equal
```

Once this feels natural, the next step—**Boolean operators `and`, `or`, and `not`**—will make much more sense because we'll start combining these individual True/False decisions.

```text
═══════════════════════ PART 10 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 11](#part-11)

---

<a id="part-11"></a>

```text
══════════════════════ PART 11 : START ═══════════════════════
```

# Part 11: Boolean Operators: and, or, not

This is the **next important piece after comparison operators**.

Comparison operators give us **one True/False decision**:

```python
age >= 18
```

→ `True` or `False`

Boolean operators let us **combine or reverse those decisions**.

There are only three:

```text
and
or
not
```

## 1. `and` — BOTH must be True

Think:

> **Condition A AND Condition B must both be true.**

Example:

```python
True and True
```

→ `True`

But:

```python
True and False
```

→ `False`

The complete truth table:

| A     | B     | `A and B` |
| ----- | ----- | --------- |
| True  | True  | **True**  |
| True  | False | **False** |
| False | True  | **False** |
| False | False | **False** |

### Easy memory

```text
AND = BOTH
```

If even **one** condition is false → result is `False`.

## Real example

Imagine:

> You can enter the server room if you have a valid ID **AND** the door is unlocked.

```python
has_id = True
door_unlocked = True

has_id and door_unlocked
```

→ `True`

But:

```python
has_id = True
door_unlocked = False
```

Then:

```python
has_id and door_unlocked
```

→ `False`

Because both conditions weren't satisfied.

## 2. `or` — At least ONE must be True

`or` asks:

> **Is at least one condition true?**

Examples:

```python
True or True
```

→ `True`

```python
True or False
```

→ `True`

```python
False or True
```

→ `True`

Only this is false:

```python
False or False
```

Truth table:

| A     | B     | `A or B`  |
| ----- | ----- | --------- |
| True  | True  | **True**  |
| True  | False | **True**  |
| False | True  | **True**  |
| False | False | **False** |

### Easy memory

```text
OR = AT LEAST ONE
```

## 3. `not` — Reverse the answer

`not` works differently.

`and` and `or` work with **two** Boolean values.

`not` works with **one**.

```python
not True
```

→ `False`

And:

```python
not False
```

→ `True`

Think:

```text
True
 ↓
not
 ↓
False
```

and:

```text
False
 ↓
not
 ↓
True
```

### Easy memory

```text
not = OPPOSITE
```

## Now combine this with comparison operators

This is where Chapter 2 starts becoming powerful.

Suppose:

```python
age = 25
```

We can ask:

```python
age >= 18
```

→ `True`

Now suppose we also have:

```python
has_id = True
```

We can combine them:

```python
age >= 18 and has_id
```

Python evaluates:

```text
age >= 18
    ↓
  True

has_id
    ↓
  True

True and True
    ↓
  True
```

So now we have:

```text
Comparison operators
        ↓
    True / False
        ↓
 Boolean operators
        ↓
 another True / False
```

That's the real purpose of `and`, `or`, and `not`.

## Do this experiment in your REPL

### `and`

```python
True and True
True and False
False and True
False and False
```

### `or`

```python
True or True
True or False
False or True
False or False
```

### `not`

```python
not True
not False
not not True
```

Then make it more realistic:

```python
age = 25
has_id = True

age >= 18 and has_id
```

Then change:

```python
has_id = False
```

and run it again.

You should see the result change.

## One small correction to the book's wording

The book says Boolean operators are used to **compare Boolean values**. At this beginner level, think of them as **combining Boolean expressions**.

For example:

```python
age >= 18 and has_id
```

Each comparison/expression produces a Boolean result, and `and` combines those results.

## Your Chapter 2 mental model

You now have:

```text
                 COMPARISON
                     │
                     ▼
              True / False
                     │
             ┌───────┼───────┐
             ▼       ▼       ▼
            and      or      not
             │       │       │
             └───────┼───────┘
                     ▼
               True / False
                     │
                     ▼
                  if/else
```

### Remember these three words

```text
AND → BOTH
OR  → AT LEAST ONE
NOT → OPPOSITE
```

That's enough for now. Don't memorize the truth tables mechanically—**run them in the REPL and observe the pattern**. Once this is comfortable, the next step is putting these Boolean results into the actual `if` statement.

```text
═══════════════════════ PART 11 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 12](#part-12)

---

<a id="part-12"></a>

```text
══════════════════════ PART 12 : START ═══════════════════════
```

# Part 12: Combining Comparison and Boolean Operators

This is where the pieces from the previous parts **start connecting together**.

You already know:

* Comparison operators → produce `True` / `False`
* `and`, `or`, `not` → work with those Boolean results

Now we combine them.

## 1. Comparison expressions produce Boolean values

Look at:

```python
4 < 5
```

This is **not itself the Boolean value** `True`.

It is an **expression that evaluates to** `True`.

```text
4 < 5
  ↓
True
```

Similarly:

```python
5 < 6
```

becomes:

```text
5 < 6
  ↓
True
```

So when we write:

```python
(4 < 5) and (5 < 6)
```

Python can think of it as:

```text
(4 < 5) and (5 < 6)
      ↓          ↓
    True       True
      ↓          ↓
       True and True
              ↓
            True
```

## 2. Let's walk through the book's second example

```python
(4 < 5) and (9 < 6)
```

First:

```text
4 < 5
 ↓
True
```

Second:

```text
9 < 6
 ↓
False
```

Now:

```text
True and False
      ↓
    False
```

Therefore:

```python
(4 < 5) and (9 < 6)
```

→ `False`

Because `and` requires **both sides to be True**.

## 3. `or` works the same way

Book example:

```python
(1 == 2) or (2 == 2)
```

First:

```text
1 == 2
  ↓
False
```

Second:

```text
2 == 2
  ↓
True
```

Then:

```text
False or True
     ↓
   True
```

Because `or` needs **at least one True**.

## 4. This is the important mental model

Don't think:

> "`and` directly compares numbers."

Instead:

```text
        COMPARISON
           ↓
      True / False
           ↓
     BOOLEAN OPERATOR
           ↓
      True / False
```

For example:

```python
age >= 18 and has_id
```

could become:

```text
age >= 18
    ↓
  True

has_id
    ↓
  True

True and True
     ↓
   True
```

## 5. Multiple operators

The book then gives this scary-looking example:

```python
spam = 4

2 + 2 == spam and not 2 + 2 == (spam + 1) and 2 * 2 == 2 + 2
```

Don't look at the whole thing at once.

We break it into pieces.

### First

```python
2 + 2 == spam
```

Since:

```text
2 + 2 → 4
spam → 4
```

we get:

```text
4 == 4
 ↓
True
```

### Second

```python
not 2 + 2 == (spam + 1)
```

First:

```text
2 + 2 → 4
spam + 1 → 5
```

So:

```text
4 == 5
 ↓
False
```

Then `not`:

```text
not False
 ↓
True
```

### Third

```python
2 * 2 == 2 + 2
```

becomes:

```text
4 == 4
 ↓
True
```

Now the whole thing is:

```text
True and True and True
             ↓
           True
```

So the final result is:

```text
True
```

## 6. Python has an order here too

Just like Chapter 1 had:

```text
**
* / // %
+ -
```

Chapter 2 has an order for these operators.

According to the book:

```text
1. Math operators
       ↓
2. Comparison operators
       ↓
3. not
       ↓
4. and
       ↓
5. or
```

So:

```python
not
```

gets evaluated before:

```python
and
```

and `and` before:

```python
or
```

### Easy memory

```text
NOT → AND → OR
```

## Why parentheses are useful

You can make the logic much easier to read:

```python
(4 < 5) and (5 < 6)
```

Instead of trying to mentally parse everything at once.

Each parenthesized comparison becomes a Boolean:

```text
(4 < 5) → True
(5 < 6) → True

True and True
     ↓
   True
```

## Your turn

Try these **without looking at the answers first**:

```python
(10 > 5) and (3 < 8)
(10 > 5) and (3 > 8)
(10 < 5) or (3 < 8)
not (10 < 5)
```

And one slightly bigger one:

```python
(5 > 2) and (10 == 10) or (3 > 8)
```

For every expression, follow this exact process:

```text
1. Solve the comparison
        ↓
2. Get True / False
        ↓
3. Apply not
        ↓
4. Apply and
        ↓
5. Apply or
        ↓
6. Final True / False
```

### The key idea from this section

> **Comparison operators create Boolean results. Boolean operators combine those results into another Boolean result.**

That's the core. Once this becomes natural, **`if` statements are going to feel much easier**, because `if` simply uses this final `True`/`False` result to decide which code should run.

```text
═══════════════════════ PART 12 : END ════════════════════════
```

[Back to contents](#contents)

---

```text
═════════════════════ END OF DAY 2 NOTES ═════════════════════
```
