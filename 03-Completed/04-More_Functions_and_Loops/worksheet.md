# 📝 Worksheet: 04 - More Functions & Loops

Do this worksheet **on paper, without a computer**. Each section ends with an **🤖 Explain It** prompt: write your explanation in your own words, then (optionally) paste it into an AI tutor and ask it to point out anything you got wrong or left out.

---

## 🧠 Section 1: Defaults and Keywords

```python
def price(amount, tax=0.0825, discount=0):
    return round(amount * (1 + tax) - discount, 2)
```

What does each call return?

1. `price(100)` → __108.25______
2. `price(100, discount=10)` → ___98.25______
3. `price(100, 0)` → ___100.00___
4. `price(discount=5, amount=20)` → ___16.65___

> 🤖 **Explain It:** Why are keyword arguments easier to read than positional ones in a call like `price(100, 0, 10)`?

Answer: keyword arguments are easier to read because they tell us what value belongs to each parameter. 
---

## 🧠 Section 2: Scope

What prints? Explain why.

```python
count = 0

def add_one(count):
    count = count + 1
    return count

result = add_one(count)
print(count, result)
```

`Answer:` ___0 1 __

> 🤖 **Explain It:** Describe the "room with two doors" model of a function in your own words.

Anwer: A function is like a separate room ,the global variables are outside the room and local variables are inside it. The fubction can receive information from outside through it parameters and send information back using return. Changing a local variable does not change the original global variable automatically. 
---

## 🧠 Section 3: Trace a While Loop

```python
balance = 100
years = 0
while balance < 150:
    balance = balance + 20
    years += 1
```

| pass | `balance` at the start | `balance < 150`? | `balance` after | `years` after |
|------|------------------------|------------------|-----------------|---------------|
| 1 | 100 | True| 120| 1|
| 2 | 120| True| 140| 2|
| 3 | 140| True| 160| 3|
| 4 | 160| False| | |

Final `years`: __3__  Final `balance`: __160_

What single change would make this an **infinite** loop?  
`Answer:` __Remove line balance = balance + 20_____

---

## 🧠 Section 4: `for` or `while`?

| Task | `for` or `while`? | Why? |
|------|-------------------|------|
| Print each line of a file | for| we iterate through each line in the file|
| Keep asking for a password until it's correct | while| we don't know how many attempts the user will need|
| Find the average of a list of prices | for | we iterate through a known list of prices|
| Double a number until it's over one million | while| we repeat until a condition becomes a false|

---

## 🧠 Section 5: Files

Given the file `pets.txt`:

```text
name,species
Rex,dog

# rescued in May
Tom,cat
```

1. How many times does `for line in f:` run? __5___
2. Which lines should your code skip, and how would you detect each one?  
   `Answer:` 
   - Line 1 (name,species) should be skipped because it is the header. Detect it by checking if the line starts with "name," or by skipping the first line
   - Line 3 (blank line): Skip using if not line.strip():
   - Line 4 (# rescued in May): Skip because it is a comment. Detect it using if line.startswith("#") after stripping whitespace____
3. Write the list of dictionaries a `read_pets` function should return:  
   `Answer:` 
    [
        {"name": "Rex", "species": "dog"},
        {"name": "Tom", "species": "cat"}   
    ]

> 🤖 **Explain It:** When is `try` / `except` a good idea, and when does it just hide bugs?

try and except are useful when something may go wrong during a program's execution. They allow the program to handle the error instead of crashing. However, it is better to catch specific errors, so we can understand and fix unexpected problems instead of ignoring them