# 📝 Worksheet: 03b - Containers → Functions

Do this worksheet **on paper, without a computer**. The point is to practice being the computer. Each section ends with an **🤖 Explain It** prompt: write your explanation in your own words, then (optionally) paste it into an AI tutor and ask it to point out anything you got wrong or left out.

---

## 🧠 Section 1: Trace a Loop

Fill in the table for this code:

```python
temps = [31, 45, 28, 52]
total = 0
count = 0
for t in temps:
    total += t
    if t <= 32:
        count += 1
```

| pass | `t` | `total` | `count` |
|------|-----|---------|---------|
| start | — | 0 | 0 |
| 1 | 31| 31| 1|
| 2 | 45| 76| 1|
| 3 | 28| 104| 2|
| 4 | 52|156 | 2|

Which **two** patterns are mixed together in this loop?  
`Answer:` ____Accumulate and count_____

> 🤖 **Explain It:** Explain the difference between Accumulate, Count, and Filter as if you were talking to someone who has never programmed.

Answer: Accumulate adds values together to get a total and count keep track of how many items meet a condition. Filter creates a new list containing only the items that meet a condition. 

---

## 🧠 Section 2: Pick Your Weapon

| Scenario | Container | Why? |
|----------|-----------|------|
| Daily step counts for a month | List| Stores daily values in order and allows changes|
| A playing card (rank, suit) | Tuple| Stores 2 related values together in a fixed structure|
| Username → password hash | Dictionary| Connects each username to its corresponding password hash|
| Every distinct IP address in a log file | Set| Automatically stores unique values without duplicates|
| Your class schedule, in order | List| Keeps classes in a specific order and can be modified|

> 🤖 **Explain It:** Pick the scenario you were *least* sure about and argue for a different container than the one you chose.

Answer: For the playing card scenario we could also use a dictionary instead of a tuple. A dictionary allows me to access each value using its name. I can also choose a tuple because a playing card has 2 fixed pieces of info that belong together. 

---

## 🧠 Section 3: Take It Apart

```python
course = {
    "title": "CMPS 3603",
    "students": [
        {"name": "Ana", "scores": [88, 92]},
        {"name": "Ben", "scores": [75, 81]}
    ]
}
```

Write the value after **each** step:

1. `course["students"]` → ____{"name": "Anna", "scores": [88, 92]} {"name": "Ben", "scores": [75, 81]}________________________
2. `course["students"][1]` → __{"name": "Ben", "scores": [75, 81]}____
3. `course["students"][1]["scores"]` → __[75, 81]____
4. `course["students"][1]["scores"][0]` → __75____

Write the expression that gets Ana's second score:  
`Answer:` __course["students"][0]["scores"][1]____

---

## 🧠 Section 4: Functions

1. What prints?

```python
def f(x):
    return x + 1

print(f(f(f(0))))
```

   `Answer:` ___3___

2. What prints? Explain why.

```python
def shout(word):
    print(word.upper())

x = shout("hi")
print(x)
```

   `Answer:` ___HI None____

3. Circle the **parameters** and underline the **arguments**:

```python
def greet(name, greeting):
    return greeting + ", " + name

greet("Ada", "Hello")
```
Answer: Parameters are name and greeting, arguments "Ada", "Hello"
> 🤖 **Explain It:** In your own words, explain `print()` vs. `return`. Then explain why getting it wrong breaks a pipeline.

Answer: print() shows infor on the screen, but it does not give a usable result to the program, while return sends a value back so another function or variable can use it

---

## 🧠 Section 5: Contracts and Tests

Write a contract for a function `longest_word(words)`:

```text
IN: A non-empty list of strings (words)
OUT: A string
DOES: Returns the longest word in the list
```

Write one test of each kind:

- Normal: ___check(longest_word(["cat", "elephant", "dog"]), "elephant")___
- Boundary: ____check(longest_word(["hello"]), "hello")____
- Weird: ____check(longest_word([]), None)__

---

## 🧠 Section 6: Draw the Pipeline

> "Given a list of prices, drop any that are `0` or less, add 8.25% tax to each, then find the total."

1. Name each function you'd need.
    
    - We need 3 functions: positive_prices(), add_tax(), calculate_total()

2. For each one, write what goes **in** and what comes **out** (shape, not just "data").

- positive_prices() : IN list of prices, OUT New list of positive prices

- add_tax() : IN list of prices, OUT New list of prices with tax

- calculate_total() : IN list of prices, OUT Number (total)


3. Draw the pipeline with arrows, labeling each arrow with the data's shape.

       prices
   (list of numbers)
          |
          v
  +------------------+
  |  positive_prices |
  +------------------+
          |
          v
        clean
   (list of numbers)
          |
          v
      +---------+
      | add_tax |
      +---------+
          |
          v
        taxed
   (list of numbers)
          |
          v
  +------------------+
  | calculate_total  |
  +------------------+
          |
          v
         total
        (number)


Code:

```python
def positive_prices(prices):
    result = []

    for price in prices:
        if price > 0:
            result.append(price)

    return result


def add_tax(prices):
    result = []

    for price in prices:
        result.append(price * 1.0825)

    return result

def calculate_total(prices):
    total = 0

    for price in prices:
        total += price

    return total

prices = [10, 25, 0, -5, 40]

clean = positive_prices(prices)
taxed = add_tax(clean)
total = calculate_total(taxed)

print(total)

```
> 🤖 **Explain It:** Why is a pipeline of small functions easier to fix than one big block of code?

Answer: Small functions are easier to fix because each one has only one responsibility. If something goes wrong, we can test each function separately and find the problem more easily instead of searching through one large block of code.
