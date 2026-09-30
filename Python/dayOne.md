# 📘 Python Notes: Basics (Chapter 1)

**Sections**
1. Math Operators
2. Order of Operations & SyntaxError
3. Data Types
4. Operators with Different Data Types
5. Variables & Assignment Statements
6. Variable Names
7. Your First Python Program

---

# 1️⃣ Section 1: Math Operators

## 🧠 The operators you need

| Operator | Meaning                |   Example | Result |
| -------- | ---------------------- | --------: | -----: |
| `**`     | Power / exponentiation |  `2 ** 3` |    `8` |
| `%`      | Remainder / modulus    |  `22 % 8` |    `6` |
| `//`     | Integer division       | `22 // 8` |    `2` |
| `/`      | Division               |  `22 / 8` | `2.75` |
| `*`      | Multiplication         |   `3 * 5` |   `15` |
| `-`      | Subtraction            |   `5 - 2` |    `3` |
| `+`      | Addition               |   `2 + 2` |    `4` |

Let's understand the **three new ones** because you already know `+ - * /`.

## `**` — Exponentiation

```python
2 ** 4
```

means:

```text
2 × 2 × 2 × 2
```

So:

```text
2 ** 4 = 16
```

Try:

```python
2 ** 3
```

You should get:

```text
8
```

## `%` — Modulus / Remainder

This one is **very important in programming**.

```python
14 % 4
```

Ask:

> When I divide 14 by 4, what is left over?

```text
14 ÷ 4

4 × 3 = 12
14 - 12 = 2
```

So `14 % 4` gives:

```text
2
```

Another:

```python
22 % 8
```

```text
8 × 2 = 16
22 - 16 = 6
```

Therefore:

```text
22 % 8 = 6
```

⚠️ **`%` here does NOT mean percentage.**

It's the **remainder operator**.

## `//` — Integer Division

Compare:

```python
25 / 8
```

Result:

```text
3.125
```

But:

```python
25 // 8
```

Result:

```text
3
```

The book describes this as division where the result is **rounded down**.

Another:

```python
29 / 10     # → 2.9
29 // 10    # → 2
```

## 🧪 Now I want you to experiment

You're already inside the Python REPL, so run these **yourself**:

```python
2 ** 4
14 % 4
25 / 8
25 // 8
```

Then make your own examples. For example:

```python
100 % 7
```

Don't ask me what the answer is first. **Predict → run → compare.**

That's how I want you to learn Python.

## 📝 Remember

```text
** → power
%  → remainder
// → integer division
/  → normal division
```

The most important one to really understand is `%`:

> **`a % b` tells you the remainder after dividing `a` by `b`.**

---

# 2️⃣ Section 2: Order of Operations & SyntaxError

> **When an expression has many operators, which part does Python calculate first?**

Python doesn't simply calculate everything from left to right.

## 🧠 The order

Remember this order for now:

```text
1️⃣ **          Power
2️⃣ * / // %    Multiplication, division, etc.
3️⃣ + -         Addition/subtraction
```

- Operators at the **same level** are calculated **left → right**.
- **Parentheses `()` can change the order.**

## 🧩 Example 1

```python
2 + 3 * 6
```

You might think:

```text
2 + 3 = 5
5 × 6 = 30
```

But Python does `*` first:

```text
3 * 6
  ↓
18

2 + 18
  ↓
20
```

So `2 + 3 * 6` → **20**

## 🧩 Parentheses can change it

```python
(2 + 3) * 6
```

Parentheses first:

```text
(2 + 3)
   ↓
  5

5 * 6
  ↓
 30
```

So `(2 + 3) * 6` → **30**

### 🧠 The important idea

```text
2 + 3 * 6   → 20
(2 + 3) * 6 → 30
```

Same numbers, same operators, but parentheses change the order.

## 🔍 The big example

```python
(5 - 1) * ((7 + 1) / (3 - 1))
```

**Step 1**

```text
(5 - 1)
   ↓
   4
```

Now:

```text
4 * ((7 + 1) / (3 - 1))
```

**Step 2**

```text
(7 + 1)
   ↓
   8
```

Now:

```text
4 * (8 / (3 - 1))
```

**Step 3**

```text
(3 - 1)
   ↓
   2
```

Now:

```text
4 * (8 / 2)
```

**Step 4**

Division:

```text
8 / 2
 ↓
4.0
```

Notice the result is `4.0`, not `4`.

Now:

```text
4 * 4.0
   ↓
16.0
```

Therefore `(5 - 1) * ((7 + 1) / (3 - 1))` → **16.0**

## ⚠️ Why `16.0` instead of `16`?

Because `/` is **normal division**, and Python produces a floating-point result.

```python
8 / 2     # → 4.0
8 // 2    # → 4
```

We'll learn more about **integers and floats** shortly. Don't overthink it now.

## 🧠 Parentheses = "Do this first"

```text
( )
 ↓
Do this first
```

Example:

```python
10 + (2 * 3)
```

Python handles:

```text
(2 * 3)
    ↓
    6

10 + 6
   ↓
  16
```

## ❌ SyntaxError

The book gives:

```python
5 +
```

Python can't understand it because you're saying:

```text
5 + ???
```

**Add 5 to what?** So:

```text
SyntaxError
```

Another:

```python
42 + 5 + * 2
```

Python sees:

```text
42 + 5 + * 2
       ↑
```

You can't put `+` and `*` together like that. Again:

```text
SyntaxError
```

## 🎯 The bigger lesson

The book's English grammar comparison is actually useful.

This:

> `This is a grammatically correct English sentence.`

has a valid structure. But:

> `This grammatically is sentence not English correct a.`

contains English words, but the **structure is wrong**.

Python is similar. `2 + 3 * 6` has valid Python structure. But `5 +` doesn't.

> **Syntax = the rules for how Python code must be structured.**

## 📝 Remember these three things

```text
Precedence  → tells Python what to calculate first.
Parentheses → let you explicitly control the order.
SyntaxError → Python cannot understand the structure of your code.
```

And bro, **definitely run these examples yourself in your REPL**:

```python
2 + 3 * 6
(2 + 3) * 6
(5 - 1) * ((7 + 1) / (3 - 1))
5 +
```

The last one is useful because you'll see a syntax error **without anything being broken**. That's exactly what the chapter wants you to become comfortable with.

---

# 3️⃣ Section 3: Data Types

## 🧠 What is a data type?

A **data type is a category that tells Python what kind of value something is.**

The three types introduced here:

```text
int   → whole numbers
float → numbers with decimal points
str   → text
```

## Integer — `int`

An integer is a **whole number**. Examples:

```python
-2
0
5
42
3298429342
```

No decimal point. So `42` is an `int`.

## Floating-point number — `float`

A float is a number containing a decimal point. Examples:

```python
3.14
0.5
42.0
-1.25
```

Important:

```text
42   → int
42.0 → float
```

Even though mathematically they represent the same quantity, **Python treats them as different data types.**

## String — `str`

A string is **text**. You put text inside quotes:

```python
"Hello"
```

or:

```python
'Hello'
```

Examples from the book:

```python
'a'
'Hello!'
'11 cats'
'5'
```

Notice this:

```text
5   ↓ number
'5' ↓ text
```

`5` is an **integer**, but `'5'` is a **string**. That's a very important distinction.

## 🔥 Connection to your earlier experiment

You previously typed:

```python
harish
```

and got:

```text
NameError
```

because Python didn't know what `harish` was. But:

```python
'harish'
```

means:

> This is a string containing the text `harish`.

Try these in your REPL:

```python
42
42.0
'42'
```

They look similar, but Python sees:

```text
42    → int
42.0  → float
'42'  → str
```

## 🧠 One subtle rule: int + float → float

```python
3 + 4
```

→ `7`. Both are integers:

```text
int + int → int
```

But:

```python
3 + 4.0
```

→ `7.0`. One value is a float:

```text
int + float → float
```

And remember: `16 / 4` gives `4.0` because `/` produces a float.

## ⚠️ Empty string

```python
''
```

is also a string. It is an **empty string**: a string containing zero characters.

```text
'hello' ↓ 5 characters
''      ↓ 0 characters
```

We'll use empty strings a lot later when working with text.

## ❌ `SyntaxError: unterminated string literal`

The book gives:

```python
'Hello, world!
```

Python sees:

```text
'
↓
Start of string

Hello, world!

???
↓
Where is the ending quote?
```

There isn't one, so Python says:

```text
SyntaxError: unterminated string literal
```

**Unterminated** basically means: *"It started, but it never got properly finished."*

Correct:

```python
'Hello, world!'
```

---

# 4️⃣ Section 4: Operators with Different Data Types

> **The same operator can behave differently depending on the data types involved.**

We already saw `+` as addition. Now Python shows us that `+` can also mean **join text**.

## `+` with numbers → Addition

```python
10 + 20
```

Python sees `int + int`:

```text
10 + 20
 ↓
30
```

## `+` with strings → Concatenation

```python
'Alice' + 'Bob'
```

Python sees `str + str`, so `+` means: **join these two strings together.**

```text
'Alice' + 'Bob'
   ↓
'AliceBob'
```

This is called **string concatenation**.

Another example:

```python
'Hello' + ' ' + 'World'
```

Result:

```text
'Hello World'
```

The middle `' '` is a string containing one space.

## ❌ String + Integer

```python
'Alice' + 42
```

Python gives:

```text
TypeError
```

Python sees `str + int`. It doesn't automatically decide: *"Oh, 42 probably means the text '42'."* You have to explicitly convert it later. For example, eventually you'll learn:

```python
'Alice' + str(42)
```

→ `'Alice42'`

**Don't worry about `str()` deeply yet.** The book explains that later.

## `*` with numbers → Multiplication

```python
5 * 4
```

Python sees `int * int`, so `5 × 4 = 20`.

## `*` with a string + integer → Replication

```python
'Alice' * 5
```

Python sees `str * int`, so `*` means: **repeat the string 5 times.**

Result:

```text
'AliceAliceAliceAliceAlice'
```

Think:

```text
'Alice' × 5

Alice
Alice
Alice
Alice
Alice
```

joined together.

## ❌ String × String

```python
'Alice' * 'Bob'
```

Doesn't make sense to Python (`str * str`):

```text
TypeError
```

## ❌ String × Float

```python
'Alice' * 5.0
```

Also doesn't work, because `5.0` is a **float**. String replication requires an **integer** count.

```text
'Alice' * 5     ✅
'Alice' * 5.0   ❌
'Alice' * 'Bob' ❌
```

## 🧠 The important pattern

| Expression  | Types       | Meaning            |
| ----------- | ----------- | ------------------ |
| `2 + 3`     | int + int   | Addition           |
| `'A' + 'B'` | str + str   | Concatenation      |
| `2 * 3`     | int × int   | Multiplication     |
| `'A' * 3`   | str × int   | String replication |
| `'A' + 3`   | str + int   | ❌ TypeError        |
| `'A' * 3.0` | str × float | ❌ TypeError        |

So the **operator alone doesn't tell the whole story**. Python looks at:

```text
Operator
   +
   ↓
What types are around it?
   ↓
Decide what operation is valid
```

## 🎯 This is why data types matter

Earlier we learned:

```text
5   → int
'5' → str
```

Now you can see why that distinction matters.

```python
5 + 5        # → 10
'5' + '5'    # → '55'
```

In the second example Python isn't doing mathematical addition. It's **joining two strings**.

## 📝 Remember

> **Python's operators can behave differently depending on the data types they operate on.**

---

# 5️⃣ Section 5: Variables & Assignment Statements

## 🧠 What is a variable?

A variable gives a **name to a value** so you can use that value later.

```python
spam = 42
```

Think:

```text
spam ─────► 42
```

You can then use `spam`:

```python
spam + 8
```

Python looks at the value associated with `spam`:

```text
spam → 42

42 + 8
 ↓
50
```

## 📦 Assignment

```python
spam = 42
```

is called an **assignment statement**. It has three parts:

```text
spam = 42
 │    │   │
 │    │   └── value
 │    └────── assignment operator
 └─────────── variable name
```

### ⚠️ Very important

The `=` here does **not** mean mathematical equality. It means:

> **Store/assign this value to this variable name.**

So `spam = 42` means: *"Make `spam` refer to the value `42`."*

## 🧪 Following the book example

**Step 1**

```python
spam = 40
```

Now:

```text
spam → 40
```

Typing `spam` gives:

```text
40
```

**Step 2**

```python
eggs = 2
```

Now:

```text
spam → 40
eggs → 2
```

So `spam + eggs` becomes:

```text
40 + 2
 ↓
42
```

**Step 3**

```python
spam + eggs + spam
```

Python gets:

```text
40 + 2 + 40
      ↓
     82
```

So: `82`

## 🔥 The important part: overwriting

```python
spam = spam + 2
```

This can look strange at first. Let's break it down.

Python first looks at the **right side**: `spam + 2`

Current value:

```text
spam → 40
```

Therefore:

```text
40 + 2
 ↓
42
```

Then Python assigns that result back to `spam`:

```text
Before:

spam → 40

After:

spam → 42
```

This is called **overwriting/reassigning the variable**.

## 🏷️ String example

The same thing works with text:

```python
spam = 'Hello'
```

```text
spam → 'Hello'
```

Then:

```python
spam = 'Goodbye'
```

```text
spam → 'Goodbye'
```

The variable's value has been replaced.

## 🧠 A small correction to the "box" analogy

The book itself says the **name-tag analogy** is better. Instead of thinking:

```text
┌─────────────┐
│ spam        │
│             │
│ 42          │
└─────────────┘
```

Think:

```text
spam ─────► 42
```

The name `spam` is associated with the value `42`. Later:

```text
spam ─────► 'Goodbye'
```

The association changed.

You don't need to understand Python's internal memory model yet. **Just use this mental model for now.**

---

# 6️⃣ Section 6: Variable Names

## 🧠 What is a variable name?

A variable name is simply the **name you give to a value**.

```python
user_name = "Harish"
```

```text
user_name → variable name
"Harish"  → value
```

A good name should tell you **what the value represents**. Instead of:

```python
x = 24
```

you could write:

```python
user_age = 24
```

When you read the code later, `user_age` immediately makes more sense.

## 📋 Python's 4 naming rules

### 1. ❌ No spaces

Wrong:

```python
user name = "Harish"
```

Correct:

```python
user_name = "Harish"
```

Use `_` when you want to separate words.

### 2. ✅ Letters, numbers and `_` are allowed

Valid:

```python
username
user_name
account4
_42
TOTAL_SUM
```

Special characters such as `$` aren't allowed:

```python
TOTAL_$UM   ❌
```

### 3. ❌ Can't start with a number

Wrong:

```python
4account = 100
```

Correct:

```python
account4 = 100
```

Also, `_42 = 42` is valid because `_` can be the first character.

### 4. ❌ Can't use Python keywords

Some words already have a special meaning in Python:

```python
if
for
return
```

You can't use them as ordinary variable names. We'll learn these words properly later.

## 🔥 Case-sensitive

This is **very important**. Python treats these as four different variable names:

```python
spam
Spam
SPAM
sPaM
```

For example:

```python
age = 20
Age = 30
```

`age` gives `20`, while `Age` gives `30`.

### 🧠 Remember

> **Python cares about uppercase and lowercase letters.**

## 🐍 Snake_case vs camelCase

The book explains two common styles.

**Snake case**

```python
user_name
current_balance
account_number
```

Words are separated using `_`.

**camelCase**

```python
userName
currentBalance
accountNumber
```

The second word starts with a capital letter.

The book explains that **both work**. Python doesn't care which style you choose. But the book has chosen **snake_case**, so for **our Python learning**, we'll use:

```python
user_name
password_file
source_ip
destination_ip
log_file
```

That's a good habit to build now.

## 🎯 One important distinction

There are two separate things:

**Python rules** determine whether the variable name is **valid**:

```python
user_name     ✅
user name     ❌
4account      ❌
```

**Naming style** determines how we **choose to write valid names**:

```python
user_name     ← snake_case
userName      ← camelCase
```

Python accepts both.

---

# 7️⃣ Section 7: Your First Python Program

Until now, you've been doing this:

```text
PowerShell
   ↓
python
   ↓
>>>
   ↓
write one instruction
   ↓
immediate result
```

That's the **interactive shell / REPL**.

Now we're learning the **file editor**.

## 🧠 REPL vs Python file

### REPL

You type:

```python
>>> print("Hello")
Hello
>>>
```

Python executes it immediately.

Good for:

* Testing a small idea
* Experimenting
* Learning syntax

### `.py` file

You create a file:

```text
hello.py
```

and put multiple instructions inside:

```python
print("Hello")
print("How are you?")
name = input(">")
print(name)
```

Then you run the **whole program**.

Think:

```text
REPL
→ one instruction at a time

.py file
→ many instructions → run the program
```

## 🎯 Since you're using VS Code

The book talks about **Mu**, but you don't need to switch to Mu. We'll do the same thing in **VS Code**.

Create a folder for our Python learning, for example:

```text
Red-Team-Journey
└── Python
    └── Chapter-01
```

Inside it create:

```text
hello.py
```

Then put the book's program into that file.

## 🔍 Let's understand the program

Don't try to understand every function yet. Some of them are concepts the book is introducing for the first time.

### 1. `print()` with a string

```python
print('Hello, world!')
```

Displays:

```text
Hello, world!
```

### 2. Displaying a question

```python
print('What is your name?')
```

Displays the question.

### 3. `input()`

```python
my_name = input('>')
```

This is important. `input()` waits for the user to type something. For example:

```text
>Harish
```

Then Python stores that text in `my_name`:

```text
my_name → "Harish"
```

### 4. String concatenation in `print()`

```python
print('It is good to meet you, ' + my_name)
```

Now we combine two strings:

```text
"It is good to meet you, "
        +
    "Harish"
```

Result:

```text
It is good to meet you, Harish
```

This connects directly to the **string concatenation** concept we just learned.

### 5. `len()`

```python
len(my_name)
```

`len()` gives the **length** of the value. If `my_name = "Al"`, then:

```text
len(my_name)
↓
2
```

We'll study `len()` properly as we continue.

### 6. `input()` returns text

```python
my_age = input('>')
```

The user enters something such as:

```text
4
```

But there is an important thing happening here that the book is about to teach: **`input()` gives you text.**

So even though you typed `4`, Python initially treats it as:

```text
"4"
```

not the integer:

```text
4
```

### 7. Converting types: `str(int(...))`

```python
str(int(my_age) + 1)
```

This is a chain:

```text
my_age
   ↓
int(my_age)
   ↓
convert text → integer
   ↓
+ 1
   ↓
str(...)
   ↓
convert result → text
```

If the user entered `4`, then:

```text
"4"
 ↓
4
 ↓
4 + 1
 ↓
5
 ↓
"5"
```

Then it can be joined with the surrounding string:

```python
'You will be ' + str(int(my_age) + 1) + ' in a year.'
```

Result:

```text
You will be 5 in a year.
```

**Don't worry if that last line looks complicated right now.** The book is intentionally showing you several concepts together, and we'll break them down one by one.

## 💡 New concept: comments

You'll see:

```python
# This program says hello and asks for my name.
```

Anything after `#` on that line is a **comment**. Python doesn't execute it as an instruction. It's there for humans to explain the code.

For example:

```python
print("Hello")  # Display a greeting
```

Python executes `print("Hello")` and ignores:

```text
# Display a greeting
```

## ▶️ Program execution

When you run `hello.py`, Python starts at the top and executes the instructions in order:

```text
Line 1
  ↓
Line 2
  ↓
Line 3
  ↓
Line 4
  ↓
...
  ↓
Last line
  ↓
Program terminates
```

**Terminates = stops running.**

Then the REPL prompt appears again:

```text
>>>
```

That means:

> The program finished, and Python is ready for another interactive instruction.

## 🧠 Your mental model now

You should now understand these as two different tools:

```text
             PYTHON
                │
       ┌────────┴────────┐
       │                 │
      REPL              .py FILE
       │                 │
   Experiment         Full program
       │                 │
   One instruction    Many instructions
       │                 │
   Immediate result    Run when needed
```

---

*End of notes.*
