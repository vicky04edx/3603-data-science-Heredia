# 📝 Worksheet: 01 - Working with Data

Use this worksheet to review and reinforce your understanding of Python's core data containers. Each section ends with an **🤖 Explain It** prompt — write your explanation in your own words, then (optionally) paste it into an AI tutor and ask it to point out anything you got wrong or left out.

---

## 🧠 Section 1: Lists

1. What method adds an item to the end of a list?  
   `Answer:` __append()____

2. How can you remove an item from a list by value? By position?  
   `Answer:` _____my_list.remove(value)________

3. What's the result of this code?

```python
nums = [2, 4, 6]
nums.append(8)
print(nums)
```

   `Answer:` ____[2, 4, 6, 8]________

4. What does `my_list[1:4]` return, given `my_list = [10, 20, 30, 40, 50]`?  
   `Answer:` _____[20, 30, 40]______

4a. Given `a = [1, 2]` and `b = [3, 4]`, what is the result of `a + b`? Of `a.append(b)`? Of `a.extend(b)`?  
   `Answer:` ____[1, 2, 3, 4] ____
   `Answer:` ____[1, 2, [3, 4]]____
   `Answer:` ____[1, 2, 3, 4]____

4b. Given `grid = [[1, 2], [3, 4]]`, how do you access the value `3`?  
   `Answer:` ___[grid[1][0]_

4c. List three different ways to remove an item from a list.  
   `Answer:` __my_list.remove(value)___
   `Answer:` __my_list.pop(index)___
   `Answer:` __del my_list[index]___

---

### ✏️ Task: List Practice

```python
# Create a list of your top 3 favorite foods.
# Add another food to the list.
# Remove one item and print the list.

foods = ["pizza", "sushi", "pasta"]
foods.append("tacos")
foods.remove("sushi")
print(foods)
```

### ✏️ Task: Slicing and Sorting

```python
# Given: numbers = [42, 17, 8, 99, 23, 4]
# 1. Print the first three numbers using slicing.
# 2. Print the numbers sorted from smallest to largest.
# 3. Print the numbers sorted from largest to smallest.

numbers = [42, 17, 8, 99, 23, 4]

print(numbers[:3])          
print(sorted(numbers))     
print(sorted(numbers, reverse=True))  
```

### ✏️ Task: Filtering

```python
# Given: temps = [55, 72, 90, 43, 88, 67, 101]
# Build a new list called "hot" containing only temps over 85.
# Print "hot".
temps = [55, 72, 90, 43, 88, 67, 101]

hot = []
for temp in temps:
    if temp > 85:
        hot.append(temp)

print(hot)  # [90, 88, 101]

```

### ✏️ Task: Combine Lists Three Ways

```python
# Given: morning = ['eggs', 'toast']
#        extras  = ['jam', 'coffee']
# 1. Use + to make a new list "breakfast" without changing "morning".
# 2. On a fresh copy of "morning", use .append(extras) and print the result.
#    Notice how many items the list has now, and why.
# 3. On another fresh copy, use .extend(extras) and print the result.

morning = ["eggs", "toast"]
extras = ["jam", "coffee"]

breakfast = morning + extras
print(breakfast)  
print(morning)    


morning_copy = morning.copy()
morning_copy.append(extras)
print(morning_copy)      
print(len(morning_copy))  

another_copy = morning.copy()
another_copy.extend(extras)
print(another_copy)       
print(len(another_copy))  
```

### ✏️ Task: 2D List (grid)

```python
# board = [
#     [1, 2, 3],
#     [4, 5, 6],
#     [7, 8, 9],
# ]
# 1. Print the value in row 2, column 0.
# 2. Change the center value to 0.
# 3. Loop over the board and print each row on its own line.
board = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
]

# 1. Row 2, column 0 (indexes start at 0).
print(board[2][0])  # 7

# 2. Change the center value.
board[1][1] = 0
print(board)
# [[1, 2, 3], [4, 0, 6], [7, 8, 9]]

# 3. Print each row on its own line.
for row in board:
    print(row)

```

### ✏️ Task: Deleting Items

```python
# Given: queue = ['Ana', 'Ben', 'Cy', 'Dana', 'Eve']
# 1. Remove 'Cy' by value.
# 2. Use .pop() to remove and capture the last person into a variable "served".
# 3. Use del to remove the first person.
# 4. Print the remaining queue and "served".
queue = ["Ana", "Ben", "Cy", "Dana", "Eve"]
queue.remove("Cy")
served = queue.pop()

del queue[0]

print(queue)  
print(served)  

```

### ✏️ Task: Iterate by Index

```python
# Given: prices = [10, 20, 30, 40]
# 1. Use "for i in range(len(prices)):" to print each item as "0: 10", "1: 20", ...
# 2. Using the index, add 5 to every price in place, then print the list.
# 3. Rewrite step 1 using enumerate() instead.

prices = [10, 20, 30, 40]

for i in range(len(prices)):
    print(f"{i}: {prices[i]}")
for i in range(len(prices)):
    prices[i] += 5
print(prices)  # [15, 25, 35, 45]
for i, price in enumerate(prices):
    print(f"{i}: {price}")
```

### 🤖 Explain It

In your own words: what's the difference between a list and a slice of a list? Is slicing a list the same as modifying it? And when you write `a.append(b)` versus `a.extend(b)`, what ends up in `a` each way?

Answer: A list is the whole collection of items, a slice is a new list containing only the selected part of the original list. 

---

## 🔒 Section 2: Tuples

5. What is a key difference between a list and a tuple?  
   `Answer:` _____A list is mutable while a tuple is not. ______

6. Can you change the contents of a tuple once it is created? Why or why not?  
   `Answer:` ______No because tuples are immutable so item assignments, additions and removals are not allowed.____

7. What does `first, *rest = (10, 20, 30, 40)` assign to `first` and `rest`?  
   `Answer:` _____first gets 10, the rest gets the list [20,30,40]____

---

### ✏️ Task: Tuple Practice

```python
# Create a tuple with your favorite 3 numbers.
# Unpack it into three variables and print each.

favorite_numbers = (7, 13, 21)

first, second, third = favorite_numbers

print(first)   
print(second)  
print(third)   
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
    ("Dallas", "Austin"): 195,
}

print(distances[("Wichita Falls", "Dallas")]) 
```

### 🤖 Explain It

In your own words: why does Python allow a tuple to be a dictionary key, but not a list? What property makes that possible?

A tuple can be used as a dictionary key because it is immutable and hashable. A list can change after it is created so it is not hashable and cannot be  used as a dictionary key. Dictionary keys need to remain stable so Python can find the correct value. 

---

## 🔑 Section 3: Dictionaries

8. What does the `.get()` method do differently from accessing a key directly with `[]`?  
   `Answer:` ___dictionary[key] raises a KeyError if the key does not exist. dictionary.get(key) returns None by default or a fallback value if you provide one. ___

9. How do you loop through both keys and values in a dictionary?  
   `Answer:` ____for key, value in my_dict.items():
    print(key, value)__

10. How would you remove a key from a dictionary and also capture the value it held?  
    `Answer:` _____removed_value = my_dict.pop("some_key")_____

11. In a list of dictionaries like `people = [{'name': 'Ana'}, {'name': 'Ben'}]`, how do you get Ben's name?  
    `Answer:` ________people[1]["name"]______

12. If the same dictionary object is stored in both a list and another dictionary, and you change it through one, does the other see the change? Why?  
    `Answer:` ______Yes, both places refer to the same dictionary object, so a change made through either reference is visible through the other.______

---

### ✏️ Task: Dictionary Practice

```python
# Create a dictionary with keys: 'name', 'age', and 'hobby'.
# Print each key and value in the format "key: value".

student = {
    "name": "Vicky",
    "age": 21,
    "hobby": "coding",
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
    "apples": {"count": 50, "price": 0.50},
    "bananas": {"count": 30, "price": 0.25},
}

for fruit, details in inventory.items():
    print(f"{fruit}: {details['count']} units at ${details['price']:.2f}")
```

### ✏️ Task: List of Dictionaries (table rows)

```python
# roster = [
#     {'name': 'Alex', 'major': 'CS'},
#     {'name': 'Ana',  'major': 'Math'},
#     {'name': 'Ben',  'major': 'History'},
# ]
# 1. Loop over roster and print "name - major" for each student.
# 2. Add a new student record to the list.
# 3. Build and print a list of just the names of everyone majoring in 'CS'.

roster = [
    {"name": "Alex", "major": "CS"},
    {"name": "Ana", "major": "Math"},
    {"name": "Ben", "major": "History"},
]

for student in roster:
    print(f"{student['name']} - {student['major']}")

roster.append({"name": "Chris", "major": "Biology"})

cs_students = []
for student in roster:
    if student["major"] == "CS":
        cs_students.append(student["name"])

print(cs_students)  # ['Alex']
```

### ✏️ Task: Update a Record by Row Number

```python
# Using the roster above:
# 1. Build a dict "by_row" mapping each row number to its record
#    (hint: {i: row for i, row in enumerate(roster)}).
# 2. Change the major of the student in row 2 to 'CS'.
# 3. Print roster[2] and explain why it changed too.

roster = [
    {"name": "Alex", "major": "CS"},
    {"name": "Ana", "major": "Math"},
    {"name": "Ben", "major": "History"},
]

by_row = {i: row for i, row in enumerate(roster)}

by_row[2]["major"] = "CS"

print(roster[2])  # {'name': 'Ben', 'major': 'CS'}
```

### 🤖 Explain It

In your own words: what's the difference between `student['gpa']` and `student.get('gpa')` when `'gpa'` isn't in the dictionary? Which would you use, and when? Also: why does changing `by_row[2]` also change `roster[2]`?

Answer: If the gpa isn’t there, using student[gpa] will throw a KeyError. But if you use student.get("gpa"), it’ll just return None instead. We grab the value with square brackets when we’re sure the key is there, and we use .get() when there's a chance the key might not be around, which helps us avoid issues.

Also, by_row[2] and roster[2] point to the same dictionary. The by_row dictionary just holds references to the existing student dictionaries, so it doesn’t create separate copies. If you change something using one reference, we’ll see the update in the other one too. 
---

## 🚀 Section 4: Going Further (Optional)

These pair with the "🔥 Challenge" sections in the notebooks — skip if you haven't gotten there yet


### ✏️ Task: List Comprehension

```python
# Rewrite this loop as a one-line list comprehension:
# cubes = []
# for n in range(6):
#     cubes.append(n ** 3)
cubes = [n ** 3 for n in range(6)]
print(cubes)  # [0, 1, 8, 27, 64, 125]
```

### ✏️ Task: namedtuple

```python
# Create a namedtuple called "Book" with fields "title" and "author".
# Make one instance and print both fields by name.

from collections import namedtuple

Book = namedtuple("Book", ["title", "author"])

my_book = Book("The Hobbit", "J.R.R. Tolkien")

print(my_book.title)  
print(my_book.author)  
```

### ✏️ Task: Word Counter

```python
# text = "to be or not to be that is the question"
# Build a dictionary counting how many times each word appears.
# (Try it by hand first, then check yourself with collections.Counter.)

text = "to be or not to be that is the question"

word_counts = {}
for word in text.split():
    if word in word_counts:
        word_counts[word] += 1
    else:
        word_counts[word] = 1

print(word_counts)

from collections import Counter
print(Counter(text.split()))
```

### ✏️ Task: Parse Some JSON

```python
import json
raw = '''
{
  "course": "Programming for Data Science",
  "online": true,
  "instructor": null,
  "students": [
    {"name": "Alex", "grade": 91},
    {"name": "Ana",  "grade": 88}
  ]
}
'''
# 1. Use json.loads(raw) to turn this into Python objects.
# 2. Print the course name and the second student's grade.
# 3. Print the Python type of the value that came from "online" and from "instructor".

import json

raw = '''
{
  "course": "Programming for Data Science",
  "online": true,
  "instructor": null,
  "students": [
    {"name": "Alex", "grade": 91},
    {"name": "Ana", "grade": 88}
  ]
}
'''

data = json.loads(raw)

print(data["course"])           
print(data["students"][1]["grade"]) 

print(type(data["online"]))     
print(type(data["instructor"]))   
```

### ✏️ Task: Walk a GeoJSON FeatureCollection

```python
geo = {
    "type": "FeatureCollection",
    "features": [
        {"type": "Feature",
         "geometry": {"type": "Point", "coordinates": [-98.529, 33.878]},
         "properties": {"name": "Bolin Hall"}},
        {"type": "Feature",
         "geometry": {"type": "Point", "coordinates": [-98.531, 33.876]},
         "properties": {"name": "Moffett Library"}},
    ],
}
# Loop over geo["features"] and print each building's name with its
# latitude and longitude. Remember: coordinates are [longitude, latitude].

for feature in geo["features"]:
    name = feature["properties"]["name"]
    longitude, latitude = feature["geometry"]["coordinates"]
    print(f"{name}: latitude {latitude}, longitude {longitude}")
```

---

## 🧾 Submit Checklist

- [x] I practiced creating, slicing, sorting, and filtering lists.
- [x] I can explain the difference between `+`, `append()`, and `extend()`.
- [x] I built and traversed a nested (2D) list.
- [x] I removed list items with `remove()`, `pop()`, and `del`.
- [x] I looped by index with `range(len(...))` and with `enumerate()`.
- [x] I understand how tuples are different from lists, and why that makes them hashable.
- [x] I accessed, looped through, updated, and removed items from a dictionary.
- [x] I built a dictionary from two separate lists.
- [x] I worked with at least one nested dictionary.
- [x] I processed a list of dictionaries as table rows and updated a record by row number.
- [x] I parsed JSON with `json.loads()` and walked a GeoJSON FeatureCollection.
- [x] I completed the "Explain It" prompts in my own words.
