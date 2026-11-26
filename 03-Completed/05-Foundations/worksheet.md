# 📝 Worksheet: 06 - Foundations

Use this worksheet to reinforce the Jupyter/IPython tooling skills from this module. There's one section per notebook, in order. Each section ends with an **🤖 Explain It** prompt. Write your explanation in your own words, then (optionally) paste it into an AI tutor and ask it to point out anything you got wrong or left out.

---

## 🧙 Section 1: Magic Commands (01)

1. What magic command lists every variable currently in memory, along with its type?  
   `Answer:` __%whos____

2. What's the difference between a line magic (`%pwd`), a cell magic (`%%writefile`), and a shell command (`!ls`)?  
   `Answer:` _%pwd is a line magic that runs a command on one line, %%writefile is a cell magic that applies to the entire cell, and !ls is a shell command that runs an operating system command.__

3. Why does `%cd ..  # go up` fail, and how do you fix it?  
   `Answer:` ___It fails because the comment is included in the magic command and may be interpreted as part of the directory path. Write %cd .. by itself, or put the comment on a separate line___

---

### ✏️ Task: Write, Run, and Reload

```python
# Use %%writefile to create tools.py containing a function shout(text)
# that returns text in uppercase with "!" on the end.
# Turn on %load_ext autoreload and %autoreload 2, then import tools and call shout().
# Rewrite tools.py so shout() adds "!!!" instead, and call it again WITHOUT restarting.

```
```python
%%writefile tools.py

def shout(text):
    return text.upper() + "!"
```

```python
%load_ext autoreload
%autoreload 2
```

```python
import tools

print(tools.shout("hello"))
```

```python
%%writefile tools.py

def shout(text):
    return text.upper() + "!!!"

```

```python

print(tools.shout("hello"))
```

### 🤖 Explain It

In your own words: why can `!pip install` "succeed" while `import` still fails with `ModuleNotFoundError`? Why does `%pip` avoid the problem?

Answer: !pip install can install a package into a different Python environment than the one used by the notebook. Therefore, the installation might succeed, but the notebook cannot find the package when using import. %pip install installs the package into the Python environment associated with the current notebook kernel, avoiding this problem.
---

## ⚡ Section 2: Jupyter Workflow (02)

4. What are the two Jupyter cell modes, and what key switches to each?  
   `Answer:` ___In Command mode we press Esc and in Edit mode we press Enter___

5. What do the numbers in `[3]`, `[4]`, `[12]` next to code cells tell you, and what *don't* they tell you?  
   `Answer:` ___They show the order in which cells were executed. They do not necessarily show the visual order of the cells or whether the notebook can run correctly from top to bottom.__

6. A cell's code ends with three expressions on three lines. How many of their values are displayed?  
   `Answer:` ____Only the last expression's value is automatically displayed.___

---

### ✏️ Task: Shortcut Drill

```
Using only the keyboard (no mouse):
- Insert 2 new cells below this one.
- Turn one into a Markdown cell and the other into a Code cell.
- Move the Code cell above the Markdown cell.
- Delete the Markdown cell, then undo the deletion.
- Run the Code cell.
```

### ✏️ Task: Break It, Then Fix It

```python
# In three separate cells, IN THIS ORDER, type:
#   print(area)
#   width, height = 4, 5
#   area = width * height
# Get it to work by running cells out of order. Then Restart & Run All,
# watch it fail, and fix the ORDER so it runs top to bottom.
```


### 🤖 Explain It

In your own words, using the "recipe and kitchen" idea: why can a notebook look correct on your screen and still fail for your grader?


Answer: A notebook is like a recipe, and the Python kernel is like a kitchen. If I already made some ingredients while testing, the notebook might work on my screen even if the recipe is written in the wrong order. My grader starts with a clean kitchen, so the notebook must have all the steps in the correct order to work
---

## 📚 Section 3: Getting Help (03)

7. What's the difference between `help(len)` and `len?`? Which one also works in a plain `.py` script?  
   `Answer:` ___help(len) displays documentation for the len function and works in a regular Python script. len? is an IPython/Jupyter shortcut that displays documentation inside a notebook__

8. In the signature `round(number, ndigits=None)`, which argument is required and which is optional? What's the optional one's default?  
   `Answer:` _number is required. ndigits is optional. Its default value is None, meaning Python rounds to the nearest whole number__

9. Name the error type for each: `'5' + 5`, `int('hello')`, `[1, 2][5]`, `{'a': 1}['b']`.  
   `Answer:` _  Type Error, Value, Error, Index Error, and Key Error____

---

### ✏️ Task: Investigate Without Searching

```python
# Using only ?, Tab/IntelliSense, dir(), and help() (no web searching):
# 1. Find a string method that checks whether a string contains only digits.
# 2. Use a wildcard search (np.*sum*?) to list NumPy functions with "sum" in the name.
# 3. Find out whether np.random.randint's upper limit is included. Prove it with code.
```

Answer:
```python
"12345".isdigit()
```

```python
np.*sum*?
```

```python
np.random.randint?
```

```python
numbers = np.random.randint(1, 2, size=10)

print(numbers)
```
### 🤖 Explain It

In your own words: when would you reach for `?` versus `??` versus `help()` versus `dir()` versus the official docs? Is there a natural order you'd try them in?

Answer: I would use ? first to quickly see an object's documentation. I would use ?? when I also want to inspect its source code, help() when I want documentation that works in regular Python, and dir() when I want to see all available methods and attributes. If I need more detailed examples or explanations, I would look at the official documentation

---

## 📝 Section 4: Markdown (04)

10. What's the difference between `*italic*` and `**bold**` in Markdown?  
    `Answer:` ___One asterisk on each side makes text italic, while two asterisks on each side make text bold.____

11. Write the Markdown for a 2-item bulleted list where the second item has its own sub-bullet.  
    `Answer:`
- First item
- Second item
  - Sub-item

12. How do you show a literal price like `$5` in Markdown without it turning into math?  
    `Answer:` _Put a backslash before the dollar sign: \$5.__

---

### ✏️ Task: Mini Report

```markdown
# Write a Markdown cell with:
# - a level-2 heading
# - a short paragraph with a line break in the middle
# - a 3-item list
# - one inline LaTeX expression (try the mean, \bar{x})
# - a small table with a right-aligned number column
```
## My Mini Report

This is a short report about studying.  
Studying every day helps improve understanding.

My favorite subjects are:

- Computer Science
- Mathematics
- English

The average of a group of values is represented by $\bar{x}$.

| Subject | Hours |
|:--------|------:|
| Math | 5 |
| Science | 8 |
| History | 3 |
### 🤖 Explain It

In your own words: what's lost if you write an entire notebook as code + comments only, with no Markdown cells at all? When would *generating* Markdown from code (`display(Markdown(...))`) be better than typing it?

ANswer: If I use only code and comments, the notebook loses its formatted explanations, headings, tables, and organized descriptions. Markdown makes the notebook easier to read and understand. Generating Markdown with display(Markdown(...)) is useful when the text needs to change automatically based on calculations or results
---

## 📂 Section 5: Files and Paths (05)

13. What does the `with` keyword do when you open a file, and why is it preferred over a plain `open()`/`.close()` pair?  
    `Answer:` ___with automatically closes a file when the block finishes, even if an error happens. It is preferred because I do not have to remember to call .close() manually.___

14. What's the difference between modes `'w'`, `'a'`, and `'x'`?  
    `Answer:` 
    'w': Creates a file or overwrites an existing file.
    'a': Adds content to the end of an existing file.
    'x': Creates a new file but raises an error if the file already exists.

15. For `Path('reports') / 'final.pdf'`, what are the `.name`, `.stem`, `.suffix`, and `.parent`?  
    `Answer:`
    ```python

    from pathlib import Path

    p = Path('reports') / 'final.pdf'

    print(p.name)    # final.pdf
    print(p.stem)    # final
    print(p.suffix)  # .pdf
    print(p.parent)  # reports`
    ```

---

### ✏️ Task: Build, Save, Reload

```python
# Create a dictionary describing yourself (name, major, hobbies as a list).
# Save it to a JSON file with indent=2 (use pathlib to build the path).
# Read it back into a NEW variable and confirm it matches the original with ==.
# Then add a tuple to the dictionary and repeat. Does it still match? Why not?


```

```python

import json
from pathlib import Path

OUT = Path('06-OutputFiles')
OUT.mkdir(exist_ok=True)

person = {
    "name": "Your Name",
    "major": "Computer Science",
    "hobbies": ["drawing", "music", "video games"]
}

json_path = OUT / 'person.json'

with open(json_path, 'w', encoding='utf-8') as file:
    json.dump(person, file, indent=2)
```

```python
with open(json_path, 'r', encoding='utf-8') as file:
    loaded_person = json.load(file)

print(person == loaded_person)
```

```python
person["location"] = (33.9, -98.5)

with open(json_path, 'w', encoding='utf-8') as file:
    json.dump(person, file, indent=2)

with open(json_path, 'r', encoding='utf-8') as file:
    loaded_person = json.load(file)

print(person == loaded_person)
```

### ✏️ Task: Find Files

```python
# Write three small .txt files into a folder using a loop.
# Use glob('*.txt') to find them WITHOUT typing their names, and print each
# file's name and size in bytes.
```

```python
folder = OUT / 'text_files'
folder.mkdir(exist_ok=True)

for i in range(1, 4):
    file_path = folder / f'file{i}.txt'

    with open(file_path, 'w', encoding='utf-8') as file:
        file.write(f'This is text file number {i}.')
```

```python
for file_path in folder.glob('*.txt'):
    print(file_path.name, file_path.stat().st_size, "bytes")
```

### 🤖 Explain It

In your own words: why does `csv.reader` give you `'90'` instead of `90`, and why does splitting a CSV line on `','` break on a value like `"Smith, John"`?
Anwer: csv.reader returns values as strings because CSV files store information as text, so the score appears as '90' instead of the number 90. Splitting a CSV line manually using ',' can also cause problems because a value like "Smith, John" contains a comma that belongs inside the name, not between two columns. The csv module understands quoted values and separates the columns correctly
---

## 📊 Section 6: Data Input and Output (06)

16. Name three things `.info()` tells you about a DataFrame that `.head()` doesn't.  
    `Answer:` _The number of rows and columns, the column names and data types, and the number of non-null (non-missing) values in each column_

17. What does `index=False` do in `df.to_csv(...)`, and what happens if you forget it?  
    `Answer:` ___It prevents Pandas from saving the DataFrame's index as an extra column. If you forget it, the index is saved and might appear as an unnecessary Unnamed: 0 column when you read the CSV again___

18. When would you use `pd.json_normalize()` instead of `pd.read_json()`?  
    `Answer:` _I would use pd.json_normalize() when JSON data contains nested dictionaries or lists and I want to flatten the information into a table with separate columns. pd.read_json() is useful for reading JSON data that already has a relatively simple tabular structure__

---

### ✏️ Task: CSV Round Trip

```python
# Create a small DataFrame from a dictionary of lists (use pd.DataFrame(your_dict)).
import pandas as pd

data = {
    "Name": ["Alice", "Bob", "Charlie"],
    "Age": [20, 22, 21],
    "Grade": [90, 85, 95]
}

df = pd.DataFrame(data)

print(df)

# Save it to a CSV, then read that CSV back into a new DataFrame.

csv_path = OUT / 'students.csv'

df.to_csv(csv_path, index=False)

loaded_df = pd.read_csv(csv_path)

print(loaded_df)
# Use .equals() to confirm the reloaded DataFrame matches the original.
print(df.equals(loaded_df))
```

### ✏️ Task: Convert Between Formats

```python
# Take the same DataFrame and save it as JSON (orient='records') and as Excel.
json_path = OUT / 'students.json'
excel_path = OUT / 'students.xlsx'

df.to_json(json_path, orient='records', indent=2)
df.to_excel(excel_path, index=False)
# Read the Excel file back and convert it to a CSV.

excel_df = pd.read_excel(excel_path)

excel_df.to_csv(OUT / 'students_from_excel.csv', index=False)

print(excel_df)
# Print the raw text of the JSON and CSV files and compare them.
with open(json_path, 'r', encoding='utf-8') as file:
    print("JSON contents:")
    print(file.read())

with open(OUT / 'students_from_excel.csv', 'r', encoding='utf-8') as file:
    print("\nCSV contents:")
    print(file.read())
```

### 🤖 Explain It

In your own words: what information can get lost or changed when converting Excel → CSV, or CSV → DataFrame? Give two examples.

Answer: Some information can be lost or changed when converting between formats. For example, Excel formatting such as cell colors, bold text, and formulas may not be preserved when converting to CSV. Also, when reading a CSV into Pandas, ZIP codes such as 02108 might become 2108 if Pandas interprets them as numbers instead of strings.
---

## 📈 Section 7: Plotting with NumPy (07)

19. What's the difference between `[1, 2, 3] * 2` and `np.array([1, 2, 3]) * 2`?  
    `Answer:` __Multiplying a Python list by 2 repeats its elements, while multiplying a NumPy array by 2 multiplies each individual element.__

20. What does `plt.legend()` require in order to show meaningful labels (hint: think about the `plot()` calls before it)?  
    `Answer:` __The plotting functions must have labels assigned using the label argument. For example, plt.plot(x, y, label="Revenue"). Then plt.legend() displays those labels.__

21. Which chart would you use for: comparing categories, change over time, a relationship between two numbers, and the distribution of one number?  
    `Answer:` 

    Comparing categories = Bar chart
    Change over time = Line chart
    Relationship between two numbers = Scatter plot
    Distribution of one number = Histogram

---

### ✏️ Task: Compare Three Categories

```python
# Given: subjects = ["Math", "Science", "History"]
#        hours_studied = [5, 8, 3]
# Build a bar plot with a title and axis labels (with units).
# Save it to a PNG file, then display it.
import matplotlib.pyplot as plt

subjects = ["Math", "Science", "History"]
hours_studied = [5, 8, 3]

# Create bar plot
plt.bar(subjects, hours_studied)

plt.title("Hours Studied by Subject")
plt.xlabel("Subject")
plt.ylabel("Hours Studied (hours)")

# Save before displaying
plt.savefig(OUT / 'study_hours.png')

plt.show()
```

### ✏️ Task: Distribution

```python
# Generate 200 test scores with np.random.normal(75, 10, size=200).
# Plot a histogram and mark the mean with a vertical dashed line and a legend.
# Then show the same data as a box plot side by side, using subplots.

import numpy as np
import matplotlib.pyplot as plt

# Generate 200 test scores
np.random.seed(1)
scores = np.random.normal(75, 10, size=200)

# Calculate mean
mean_score = np.mean(scores)

# Create two plots side by side
fig, axes = plt.subplots(1, 2, figsize=(12, 5))

# Histogram
axes[0].hist(scores, bins=15, edgecolor='black', label='Test Scores')
axes[0].axvline(mean_score, color='red', linestyle='--',
                label=f'Mean = {mean_score:.2f}')

axes[0].set_title('Distribution of Test Scores')
axes[0].set_xlabel('Score')
axes[0].set_ylabel('Frequency')
axes[0].legend()

# Box plot
axes[1].boxplot(scores)
axes[1].set_title('Box Plot of Test Scores')
axes[1].set_ylabel('Score')

plt.tight_layout()
plt.savefig(OUT / 'score_distribution.png')
plt.show()
```

### 🤖 Explain It

In your own words: why does a line plot make sense for `sin(x)` or monthly revenue but not for the categories `"Math"`, `"Science"`, `"History"`? What's different about the two kinds of data?

Answer: A line plot makes sense for sin(x) or monthly revenue because the values have a natural order or continuous relationship. Categories like Math, Science, and History are separate groups and do not have a continuous relationship between them. A bar chart is better for comparing those categories.
---

## ⏱️ Section 8: Timing and Performance (08)

22. What's the difference between `%time`, `%timeit`, `%%time`, and `%%timeit`?  
    `Answer:` 
    %time: Measures execution time for one line, usually one execution
    %timeit: Repeatedly measures one line and reports timing statistics
    %%time: Measures the execution time of an entire cell
    %%timeit: Repeatedly measures the execution time of an entire cell

23. Name two things that make a timing comparison **unfair**.  
    `Answer:` __
    
    1. Including setup work in one measurement but not the other, such as creating a NumPy array inside only one timed operation.
    2. Comparing different amounts of work or different input sizes.___

24. What does `%prun` tell you that `%timeit` doesn't?  
    `Answer:` ___%prun shows how much time is spent inside individual functions called by the code. %timeit measures the overall execution time but does not explain which functions take the most time.__

---

### ✏️ Task: Timing Comparison

```python
# Build a list of the first 500,000 integers, and a NumPy array of the same.
# Use %timeit -o to compare squaring the list with a comprehension
# vs. squaring the array (numbers ** 2).
# Confirm both give the same values, then print how many times faster NumPy is.

numbers_list = list(range(500_000))
numbers_array = np.array(numbers_list)

list_result = [x ** 2 for x in numbers_list]
array_result = numbers_array ** 2

print("Results are equal:",
      np.array_equal(list_result, array_result))

      
list_time = %timeit -o [x ** 2 for x in numbers_list]

numpy_time = %timeit -o numbers_array ** 2
speedup = list_time.average / numpy_time.average

print(f"NumPy is approximately {speedup:.2f} times faster.")

```

### ✏️ Task: Find the Repeated Work

```python
# This function is slow. Time it, find the repeated work, fix it, and time it again:
def above_average(values):
    return [v for v in values if v > sum(values) / len(values)]
```

```python
values = list(range(5000))

slow_time = %timeit -o -n 1 -r 3 above_average(values)
```

```python
def above_average_fast(values):
    average = sum(values) / len(values)

    return [v for v in values if v > average]
```

```python
print(above_average(values) == above_average_fast(values))
```

```python
fast_time = %timeit -o -n 1 -r 3 above_average_fast(values)

speedup = slow_time.average / fast_time.average

print(f"The improved function is approximately {speedup:.2f} times faster.")
```

### 🤖 Explain It

In your own words: why does `%timeit` run your code many times instead of just once like `%time`? And why should you "measure first" instead of rewriting code that just *looks* slow?

Answer: %timeit runs code multiple times because one measurement might be affected by other computer activities or temporary delays. Repeating the measurement gives a more reliable estimate of execution time. I should measure first because code that looks slow is not always the actual problem. Profiling and timing help me find the real bottleneck before changing the code
---

## 🧾 Submit Checklist

- [x] I used at least 3 different magic commands, including one cell magic and one `!` shell command.
- [x] I fixed a notebook that only worked when run out of order, and I can explain why it failed.
- [x] I ran **Restart & Run All** on my notebooks before submitting.
- [x] I used `?`, `help()`, or `dir()` to look something up instead of guessing.
- [x] I wrote a Markdown cell with a heading, a list, a table, and LaTeX math.
- [x] I wrote and read back a text file, a JSON file, and a CSV file using `pathlib` paths.
- [x] I loaded data with Pandas and converted it between at least two formats.
- [x] I built at least one plot with a title, axis labels, and (if applicable) a legend, and saved it to a file.
- [x] I timed two approaches fairly with `%timeit`.
- [x] I uploaded each notebook's `NN-OutputFiles` folder along with the notebook.
- [x] I completed the "Explain It" prompts in my own words.
