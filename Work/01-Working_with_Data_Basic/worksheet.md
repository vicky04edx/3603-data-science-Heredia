# 📝 Worksheet: 01 - Working with Data

Use this worksheet to review and reinforce your understanding of Python's core data containers. Each section ends with an **🤖 Explain It** prompt — write your explanation in your own words, then (optionally) paste it into an AI tutor and ask it to point out anything you got wrong or left out.

---

## 🧠 Section 1: Lists

1. What method adds an item to the end of a list?  
   `Answer:` ___.append()___________

2. How can you remove an item from a list by value? By position?  
   `Answer:` ___By values use .remove(value) and by index use .pop(index)_____

3. What's the result of this code?

```python
nums = [2, 4, 6]
nums.append(8)
print(nums)
```

   `Answer:` ______[2, 4, 6, 8]_______

4. What does `my_list[1:4]` return, given `my_list = [10, 20, 30, 40, 50]`?  
   `Answer:` ____[2, 4, 6, 8]_______

---

### ✏️ Task: List Practice

```python
# Create a list of your top 3 favorite foods.
   foods = ["pizza", "tacos", "pasta"]
# Add another food to the list.
   foods.append("sushi")
# Remove one item and print the list.
   foods.remove("tacos")

   print(foods)

```

### ✏️ Task: Slicing and Sorting

```python
# Given: numbers = [42, 17, 8, 99, 23, 4]
   numbers = [2, 17, 8, 99, 23, 4]
# 1. Print the first three numbers using slicing.
   print(numbers[:3])
# 2. Print the numbers sorted from smallest to largest.
   print(sorted(numbers))
# 3. Print the numbers sorted from largest to smallest.
   print(sorted(numbers, reverse=True))
```

### ✏️ Task: Filtering

```python
# Given: temps = [55, 72, 90, 43, 88, 67, 101]
# Build a new list called "hot" containing only temps over 85.
temps = [55, 72, 90, 43, 88, 67, 101]
hot = []

for temp in temps:
   if temp > 85:
      hot.append(temp)

# Print "hot".
   print(hot)
```

### 🤖 Explain It

In your own words: what's the difference between a list and a slice of a list? Is slicing a list the same as modifying it?

A list is the entire collection of times. A slice is a smaller portion of that list that we select using [start:stop]

---

## 🔒 Section 2: Tuples

5. What is a key difference between a list and a tuple?  
   `Answer:` __Lists are muatble while tuples are immutable___

6. Can you change the contents of a tuple once it is created? Why or why not?  
   `Answer:` _Tuples are immutable meaning that once they are created their elements can't be changed, added, ore removed__

7. What does `first, *rest = (10, 20, 30, 40)` assign to `first` and `rest`?  
   `Answer:` __first = 10 and rest = [20, 30, 40]__

---

### ✏️ Task: Tuple Practice

```python
# Create a tuple with your favorite 3 numbers.
# Unpack it into three variables and print each.

# Create a tuple with your favorite 3 numbers.
numbers = (7, 21, 42)

# Unpack it into three variables.
a, b, c = numbers

print(a)
print(b)
print(c)
```

### ✏️ Task: Unpacking with *rest

```python
# Given: race_times = (9.58, 9.63, 9.69, 9.71, 9.74)
# Unpack this into "winner" (the first time) and "others" (everything else).
# Print both.
race_times = (9.58, 9.63, 9.69, 9.71, 9.74)
winner, *others = race_times

print(winner)
print(others)

```

### ✏️ Task: Tuples as Dictionary Keys

```python
# Build a dictionary called "distances" where the keys are (city1, city2)
# tuples and the values are the distance in miles between them.
# Add at least two entries, then look up and print one of them.
distances = {
    ("Wichita Falls", "Dallas"): 140,
    ("Dallas", "Austin"): 195
}

print(distances[("Wichita Falls", "Dallas")])

```

### 🤖 Explain It

In your own words: why does Python allow a tuple to be a dictionary key, but not a list? What property makes that possible?

A tuple can be a dictionary keu because tuples are immutable. Dictionary keys need to be hashable which means that Python can calculate a stable hash value for them. Lists are mutable so they are not hashable and can't be dictionary keys.

---

## 🔑 Section 3: Dictionaries

8. What does the `.get()` method do differently from accessing a key directly with `[]`?  
   `Answer:` ____brackets give you and error and .get() gives you a value____

9. How do you loop through both keys and values in a dictionary?  
   `Answer:` ___use .items()____

10. How would you remove a key from a dictionary and also capture the value it held?  
    `Answer:` __using .pop()____

---

### ✏️ Task: Dictionary Practice

```python
# Create a dictionary with keys: 'name', 'age', and 'hobby'.
# Print each key and value in the format "key: value".

student = {
    "name": "Ana",
    "age": 20,
    "hobby": "reading"
}

for key, value in student.items():
    print(f"{key}: {value}")
```

### ✏️ Task: Build from Two Lists

```python
# Given: products = ['pen', 'notebook', 'eraser']
#        prices = [1.50, 3.25, 0.75]
# Build a dictionary mapping each product to its price.
# Print the total cost of all products (hint: sum the .values()).

products = ["pen", "notebook", "eraser"]
prices = [1.50, 3.25, 0.75]

products = ["pen", "notebook", "eraser"]
prices = [1.50, 3.25, 0.75]

product_prices = dict(zip(products, prices))

print(product_prices)

total = sum(product_prices.values())

print(total)
```

### ✏️ Task: Nested Dictionaries

```python
# Given:
# inventory = {
#     'apples': {'count': 50, 'price': 0.50},
#     'bananas': {'count': 30, 'price': 0.25},
# }
# Loop through inventory and print a line for each fruit like:
# "apples: 50 units at $0.50"
inventory = {
    'apples': {'count': 50, 'price': 0.50},
    'bananas': {'count': 30, 'price': 0.25},

    for fruit, info in inventory.items():
    print(f"{fruit}: {info['count']} units at ${info['price']:.2f}")
}



```

### 🤖 Explain It

In your own words: what's the difference between `student['gpa']` and `student.get('gpa')` when `'gpa'` isn't in the dictionary? Which would you use, and when?

If 'gpa' is not in the dictionary, it causes a KeyError but student.get('gpa') return None instead

---

## 🚀 Section 4: Going Further (Optional)

These pair with the "🔥 Challenge" sections in the notebooks — skip if you haven't gotten there yet.

### ✏️ Task: List Comprehension

```python
# Rewrite this loop as a one-line list comprehension:
# cubes = []
# for n in range(6):
#     cubes.append(n ** 3)
```

### ✏️ Task: namedtuple

```python
# Create a namedtuple called "Book" with fields "title" and "author".
# Make one instance and print both fields by name.
```

### ✏️ Task: Word Counter

```python
# text = "to be or not to be that is the question"
# Build a dictionary counting how many times each word appears.
# (Try it by hand first, then check yourself with collections.Counter.)
```

---

## 🧾 Submit Checklist

- [ ] I practiced creating, slicing, sorting, and filtering lists.
- [ ] I understand how tuples are different from lists, and why that makes them hashable.
- [ ] I accessed, looped through, updated, and removed items from a dictionary.
- [ ] I built a dictionary from two separate lists.
- [ ] I worked with at least one nested dictionary.
- [ ] I completed the "Explain It" prompts in my own words.
