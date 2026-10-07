# Python Learning Notes: Chapter 3 Loops

Chapter 3 starts with loops: the `while` loop, `break`, `continue`, and truthy/falsey values, each with trace-it-yourself challenges.

<a id="contents"></a>

## Contents

1. [Part 1: The while Loop](#part-1)
2. [Part 2: An Annoying while Loop](#part-2)
3. [Part 3: The break Statement](#part-3)
4. [Part 4: The continue Statement](#part-4)
5. [Part 5: Truthy and Falsey Values](#part-5)
6. [Part 6: Recap and Challenge: break, continue, Truthy/Falsey](#part-6)

---

<a id="part-1"></a>

```text
═══════════════════════ PART 1 : START ═══════════════════════
```

# Part 1: The while Loop

You already know that `if` means: check a condition **once**. A `while` loop means:

> Keep checking the condition and keep executing the block while it is `True`.

## `if` vs `while`

The book gives almost the same code for both.

### `if`

```python
spam = 0
if spam < 5:
    print('Hello, world.')
    spam = spam + 1
```

Python does:

```text
spam = 0
    ↓
spam < 5 ?
    ↓
True
    ↓
print
    ↓
spam = 1
    ↓
continue after if
    ↓
END
```

It checks the condition once. Output:

```text
Hello, world.
```

Only once.

### `while`

```python
spam = 0
while spam < 5:
    print('Hello, world.')
    spam = spam + 1
```

The beginning looks almost identical:

```text
spam = 0
    ↓
spam < 5 ?
```

But when Python reaches the bottom of the block, it **jumps back**:

```text
             ┌──────────────────┐
             │                  ↓
spam = 0 → spam < 5 ? ──True──→ print()
             ↑                   ↓
             │              spam = spam + 1
             │                   │
             └───────────────────┘
                     │
                   False
                     ↓
                    End
```

That's the loop.

## Trace Every Iteration

Code:

```python
spam = 0
while spam < 5:
    print('Hello, world.')
    spam = spam + 1
```

**Iteration 1**

```text
spam = 0
0 < 5 → True
```

Print `Hello, world.` Then:

```text
spam = 0 + 1
spam = 1
```

**Iteration 2**

Python goes back to `while spam < 5:`. Now:

```text
1 < 5 → True
```

Print. Then `spam = 2`.

**Iteration 3**

```text
2 < 5 → True
```

Print. Then `spam = 3`.

**Iteration 4**

```text
3 < 5 → True
```

Print. Then `spam = 4`.

**Iteration 5**

```text
4 < 5 → True
```

Print. Then `spam = 5`.

**One final check**

Python goes back again:

```text
5 < 5 → False
```

Now the `while` block is skipped. END.

So the output is `Hello, world.` printed 5 times:

```text
Hello, world.
Hello, world.
Hello, world.
Hello, world.
Hello, world.
```

## The Most Important Concept: Iteration

The book introduces the word **iteration**.

> An iteration is one complete execution of the loop's block.

So here:

```python
while spam < 5:
    print('Hello, world.')
    spam = spam + 1
```

we have:

```text
Iteration 1 → spam 0 → 1
Iteration 2 → spam 1 → 2
Iteration 3 → spam 2 → 3
Iteration 4 → spam 3 → 4
Iteration 5 → spam 4 → 5
```

Then:

```text
5 < 5 → False
```

The loop ends.

## Very Important: Something Must Change

Look at this line:

```python
spam = spam + 1
```

It changes the value of `spam`. Without it:

```python
spam = 0
while spam < 5:
    print('Hello, world.')
```

What happens?

```text
spam = 0
   ↓
0 < 5 → True
   ↓
print
   ↓
spam is STILL 0
   ↓
0 < 5 → True
   ↓
print
   ↓
spam is STILL 0
   ↓
...
```

It would keep running because the condition never becomes `False`. That's the beginning of understanding **infinite loops**.

## Mental Model

For every `while` loop, ask:

```text
1. What is the starting value?
          ↓
2. What condition is checked?
          ↓
3. What happens inside the loop?
          ↓
4. What changes?
          ↓
5. When will the condition become False?
```

This matters later when writing automation and security scripts, where you constantly need to reason about loops.

## Your First Challenge

Don't run this yet:

```python
spam = 0
while spam < 3:
    print('Hello')
    spam = spam + 1
```

Trace it manually and fill in:

```text
Iteration 1:
spam = ?
condition = ?
output = ?
spam becomes = ?

Iteration 2:
spam = ?
condition = ?
output = ?
spam becomes = ?

Iteration 3:
spam = ?
condition = ?
output = ?
spam becomes = ?

Final condition:
spam = ?
condition = ?
```

Don't run the code. Think like Python first.

```text
════════════════════════ PART 1 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 2](#part-2)

---

<a id="part-2"></a>

```text
═══════════════════════ PART 2 : START ═══════════════════════
```

# Part 2: An Annoying while Loop

Now the book shows a real `while` loop where the value comes from the user. Understand the logic rather than just memorizing the code.

The code:

```python
name = ''
while name != 'your name':
    print('Please type your name.')
    name = input('>')
print('Thank you!')
```

## What Is Happening?

The loop is basically asking: *"Is the name NOT your name?"*

- If yes: keep asking.
- If no: stop asking.

```text
             name != 'your name'?
                    │
             ┌──────┴──────┐
           True           False
             │               │
             ↓               ↓
       ask for name      Thank you!
             │
             ↓
       user enters name
             │
             └──────────────→ check again
```

### Step 1: Why `name = ''`?

At the beginning:

```python
name = ''
```

`''` is an empty string. Now Python checks `name != 'your name'`, which becomes:

```text
'' != 'your name'
```

That's `True`, so Python enters the loop.

### Step 2: User enters something wrong

Suppose the user types `Al`. Now `name = 'Al'`. Python goes back to the condition:

```text
'Al' != 'your name'  →  True
```

So the loop runs again.

### Step 3: User enters another wrong value

The user types `Albert`:

```text
'Albert' != 'your name'  →  True
```

Loop again.

### Step 4: User finally enters the expected value

The user types `your name`. Now `name = 'your name'` and Python checks:

```text
'your name' != 'your name'  →  False
```

This is the important moment. Because the condition is `False`, Python does not enter the loop again. It moves to:

```python
print('Thank you!')
```

## We Don't Know How Many Times It Will Run

Unlike the previous example (`while spam < 3:`, exactly 3 iterations), here:

```python
while name != 'your name':
```

we don't know how many iterations there will be. It depends on the user's input. For example:

```text
Al
↓
Albert
↓
Harish
↓
hello
↓
something
↓
your name
```

The loop keeps going until the condition becomes `False`.

## Infinite Loop Again

The book points out: if you never enter the required name, the condition never becomes `False`. For example:

```text
> Al
> Bob
> Harish
> Test
> Python
> Linux
> Cybersecurity
> ...
```

Every time:

```text
input != 'your name'
       ↓
      True
       ↓
loop again
```

So the loop can continue forever. That's called an **infinite loop**.

## Compare the Two Loops

### Previous loop

```python
spam = 0
while spam < 3:
    print('Hello')
    spam = spam + 1
```

The program itself changes the condition:

```text
0 → 1 → 2 → 3
```

Eventually:

```text
3 < 3 → False
```

### This loop

```python
name = ''
while name != 'your name':
    name = input('>')
```

The user changes the condition by providing a new value:

```text
'' → 'Al' → 'Albert' → 'your name'
```

Eventually:

```text
'your name' != 'your name'
                  ↓
                False
```

## Remember

A `while` loop keeps running as long as its condition is `True`. Every iteration must eventually have a way to make the condition `False`, otherwise the loop may continue forever.

```text
while
 ↓
condition
 ↓
True
 ↓
execute block
 ↓
change something
 ↓
check condition again
 ↓
False
 ↓
exit loop
```

## Your Turn

Don't run this yet:

```python
name = ''
while name != 'Harish':
    print('Enter your name:')
    name = input('>')
print('Welcome!')
```

Imagine the user enters:

```text
> Alex
> Bob
> Harish
```

Trace it iteration by iteration. This time, trace the user input changing the condition:

```text
Starting name = ?

Iteration 1:
condition = ?
user enters = ?
new name = ?

Iteration 2:
condition = ?
user enters = ?
new name = ?

Iteration 3:
condition = ?
user enters = ?
new name = ?

Final:
condition = ?
What happens?
```

```text
════════════════════════ PART 2 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 3](#part-3)

---

<a id="part-3"></a>

```text
═══════════════════════ PART 3 : START ═══════════════════════
```

# Part 3: The break Statement

The main idea is simple: **`break` immediately exits the current loop.**

The example:

```python
while True:
    print('Please type your name.')
    name = input('>')
    if name == 'your name':
        break
print('Thank you!')
```

## First Understand `while True`

Normally we have something like:

```python
while spam < 3:
```

The condition can eventually become `False`. But here:

```python
while True:
```

the condition is simply `True`, and `True` is always `True`:

```text
while True
    ↓
True
    ↓
enter loop
    ↓
reach bottom
    ↓
check again
    ↓
True
    ↓
enter loop again
    ↓
...
```

By itself, this is an infinite loop.

## So How Do We Escape?

The program has:

```python
if name == 'your name':
    break
```

Think of `break` as an **emergency exit door**.

```text
while True
     │
     ↓
Ask for name
     │
     ↓
name == 'your name'?
     │
 ┌───┴────┐
True     False
 │         │
 ↓         ↓
break    iteration ends
 │         │
 ↓         └──────→ go back to while
exit loop
 │
 ↓
Thank you!
```

## Trace It

Suppose the user enters `Alex`. Python checks `name == 'your name'`:

```text
'Alex' == 'your name'
       ↓
     False
```

Therefore `break` is skipped. Python reaches the bottom of the loop. Because `while True:` is still `True`, it goes back to the beginning.

User enters `Bob`. Again:

```text
'Bob' == 'your name'
       ↓
     False
```

No `break`. Loop again.

Finally, the user enters `your name`:

```text
'your name' == 'your name'
             ↓
            True
```

Therefore Python executes `break` and immediately leaves the `while` loop. Then Python continues with:

```python
print('Thank you!')
```

## Compare the Two Approaches

### Previous program

```python
while name != 'your name':
    name = input('>')
```

The `while` condition controls when the loop stops:

```text
condition becomes False
          ↓
       exit loop
```

### `break` version

```python
while True:
    name = input('>')
    if name == 'your name':
        break
```

The `while` condition itself never becomes `False`. Instead:

```text
condition always True
        ↓
     keep looping
        ↓
special condition
        ↓
      break
        ↓
    exit loop
```

That's the key difference.

## Important Mental Model

Don't think: *"`break` makes the condition False."* That's not what happens. Instead:

```text
while True
   ↓
condition is still True
   ↓
break encountered
   ↓
EXIT LOOP IMMEDIATELY
```

> `break` changes the execution path, not the value of the `while` condition.

## Why Would We Use This?

Sometimes the stopping condition is easier to check inside the loop. For example:

```python
while True:
    get some input
    if something is correct:
        break
```

This gives us a simple structure:

```text
Keep doing this
      ↓
until this special condition happens
      ↓
break
```

## One Warning from the Book

This:

```python
while True:
```

is an infinite loop unless something inside eventually executes `break`. If you accidentally write:

```python
while True:
    print('Hello')
```

there is no exit. It will keep printing. So when you see `while True:`, your first question should be:

> "Where is the `break` that gets me out?"

That's a very useful habit.

## Your Turn

Don't run this yet:

```python
while True:
    number = input('Enter 5: ')
    if number == '5':
        break
print('Correct!')
```

Imagine the user enters:

```text
2
8
hello
5
```

Trace it:

```text
Input 1:
number = ?
condition = ?
break? → Yes / No

Input 2:
number = ?
condition = ?
break? → Yes / No

Input 3:
number = ?
condition = ?
break? → Yes / No

Input 4:
number = ?
condition = ?
break? → Yes / No

After break:
What does Python execute?
```

Don't run it. Trace it like Python first.

```text
════════════════════════ PART 3 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 4](#part-4)

---

<a id="part-4"></a>

```text
═══════════════════════ PART 4 : START ═══════════════════════
```

# Part 4: The continue Statement

This is easier if you connect it directly with `break`.

## What Is It?

`continue` means:

> Stop the current iteration and immediately go back to the start of the loop.

```text
break
  ↓
EXIT the loop

continue
  ↓
SKIP rest of current iteration
  ↓
GO BACK to loop condition
```

That is the biggest difference.

## `break` vs `continue`

Imagine:

```python
while True:
    ...
```

**`break`**

```text
loop
 ↓
break
 ↓
EXIT LOOP
 ↓
continue after loop
```

**`continue`**

```text
loop
 ↓
continue
 ↓
SKIP remaining code
 ↓
GO BACK TO LOOP START
```

So remember:

- **`break`** = I'm done with the loop.
- **`continue`** = I'm done with this iteration.

## The Book's `swordfish.py`

The important part:

```python
while True:
    print('Who are you?')
    name = input('>')
    if name != 'Joe':
        continue
    print('Hello, Joe. What is the password? (It is a fish.)')
    password = input('>')
    if password == 'swordfish':
        break
print('Access granted.')
```

Let's follow the execution.

### User enters a wrong name

Suppose:

```text
Who are you?
> Harish
```

Now `if name != 'Joe':` becomes:

```text
'Harish' != 'Joe'
        ↓
       True
```

So `continue` executes. Python doesn't execute `print('Hello, Joe. What is the password?')`. It immediately jumps back:

```text
continue
   ↓
START OF WHILE
   ↓
Who are you?
```

So the user isn't even asked for the password.

### User enters Joe

```text
> Joe
```

Now:

```text
'Joe' != 'Joe'
      ↓
    False
```

Therefore `continue` is skipped. Python moves to `print('Hello, Joe. What is the password?')` and then asks for the password.

### Wrong password

Suppose:

```text
> Mary
```

Check `if password == 'swordfish':`:

```text
'Mary' == 'swordfish'
       ↓
     False
```

So `break` doesn't run. Python reaches the end of the `while` block, then naturally goes back to the start:

```text
end of iteration
       ↓
while True
       ↓
Who are you?
```

### Correct password

The user enters:

```text
>swordfish
```

Now:

```text
'swordfish' == 'swordfish'
             ↓
            True
```

So `break` runs. That exits the loop completely. Then `print('Access granted.')` gives:

```text
Access granted.
```

## The Full Flow

```text
                 while True
                     ↓
                Ask for name
                     ↓
             name == 'Joe'?
                /          \
             No              Yes
             ↓                ↓
         continue        Ask password
             ↓                ↓
      back to start     password correct?
                         /          \
                       No            Yes
                       ↓              ↓
                  loop again       break
                                      ↓
                              Access granted
```

That's the exact logic from the book.

## Infinite Loop Emergency

The book also shows:

```python
while True:
    print('Hello, world!')
```

This has no `break`, so it keeps going forever. If you accidentally run something like this in your terminal, press:

```text
Ctrl + C
```

It sends a `KeyboardInterrupt` and stops the program. In VS Code (rather than Mu), you can also use the terminal's interrupt/stop controls.

```text
════════════════════════ PART 4 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 5](#part-5)

---

<a id="part-5"></a>

```text
═══════════════════════ PART 5 : START ═══════════════════════
```

# Part 5: Truthy and Falsey Values

Python doesn't always require you to explicitly write `== True`.

The book says these values are considered `False` in conditions:

```text
0
0.0
''
```

Other values are considered `True`. For example:

```python
bool(0)        # False
bool(42)       # True
bool('Hello')  # True
bool('')       # False
```

## Why Does This Matter?

Look at the book's example:

```python
name = ''
while not name:
    print('Enter your name:')
    name = input('>')
print('How many guests will you have?')
num_of_guests = int(input('>'))
if num_of_guests:
    print('Be sure to have enough room for all your guests.')
print('Done')
```

At first `name = ''`. An empty string is falsey. So `not name` means:

```text
name → ''
       ↓
    False
       ↓
   not False
       ↓
      True
```

Therefore the loop runs. Once the user enters a name:

```text
name → 'Harish'
```

A non-empty string is truthy:

```text
'Harish' → True
```

Therefore:

```text
not True → False
```

and the loop stops.

```text
════════════════════════ PART 5 : END ════════════════════════
```

[Back to contents](#contents) | [Next: Part 6](#part-6)

---

<a id="part-6"></a>

```text
═══════════════════════ PART 6 : START ═══════════════════════
```

# Part 6: Recap and Challenge: break, continue, Truthy/Falsey

## Remember These Three Things

**`break`**

```text
EXIT LOOP
```

**`continue`**

```text
SKIP REST OF CURRENT ITERATION
→ GO BACK TO LOOP START
```

**Truthy / Falsey**

```text
0       → False
0.0     → False
''      → False

other values → True
```

## Your Challenge

Don't run this yet:

```python
while True:
    number = int(input('Enter a number: '))
    if number == 0:
        continue
    if number == 5:
        break
    print('You entered:', number)
print('Finished')
```

The user enters:

```text
2
0
7
5
```

Trace exactly what happens. Pay special attention to the difference between `continue` and `break`:

```text
Input: 2
→ ?

Input: 0
→ ?

Input: 7
→ ?

Input: 5
→ ?

Final output:
→ ?
```

```text
════════════════════════ PART 6 : END ════════════════════════
```

[Back to contents](#contents)

---

```text
════════════════ END OF CHAPTER 3 LOOPS NOTES ════════════════
```
