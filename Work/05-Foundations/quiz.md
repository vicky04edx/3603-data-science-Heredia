# 📝 Quiz: 05 - Foundations

45 questions covering all 8 Foundations notebooks, in order: magic commands, Jupyter workflow, getting help, Markdown, files and paths, data I/O, plotting with NumPy, and timing. Mix of multiple choice, true/false, code-tracing, and short answer.

This is a conceptual self-check. For hands-on practice writing and running real code, see `Foundations_Quiz.ipynb` in this folder.

Try every question on your own first. The Answer Key is at the bottom, no peeking. If you're using an AI tutor, paste in *your answer* and ask it to check your reasoning rather than asking it to solve the question first.

---

## Section A — Magic Commands (01)

**1.** (Multiple Choice) Which magic command shows your current working directory?
A) `%pwd`  B) `%cd`  C) `%ls`  D) `%dir`

Answer: A

**2.** (Short Answer) What's the difference between a line magic (`%pwd`), a cell magic (`%%writefile`), and a shell command (`!ls`)?

Answer: %pwd runs a command on one line in a notebook and %%writefile applies to the entire cell, usually affecting all its contents. !ls runs a command in the operating system's terminal.

**3.** (Code Tracing) A folder contains only `a.csv` and `b.txt`. What kind of value is stored in `files`, and what does it contain?
```python
files = !ls
```
Answer: files store a llist-like Ipython result containing the filenames: ['a.csv', 'b.txt']

**4.** (True/False) `%cd ..  # go up one folder` moves you to the parent folder.

Answer: True

**5.** (Short Answer) Inside a notebook, why should you install packages with `%pip install` instead of `!pip install`?

Answer: %pip install installs packages into the Python environment used by the current notebook kernel. !pip install might use a different Python environment, which could cause the notebook not to find the installed package

**6.** (True/False) `%who` only shows variable *names*, while `%whos` also shows their types and a value summary.
Answer: True
---

## Section B — Jupyter Workflow (02)

**7.** (Multiple Choice) In Command mode, which keyboard shortcut inserts a new cell *below* the current one?
A) `A`  B) `B`  C) `D D`  D) `M`
Answer: B
**8.** (Short Answer) What's the practical difference between Command mode and Edit mode? You press `B` and the letter "b" appears in your code. What happened, and what should you press?

Answer: Command mode is used to manage cells, such as adding, deleting, and moving them. Edit mode is sed to write and modify code inside a cell.

**9.** (Code Tracing) What does this cell display when it runs?
```python
x = 5
x * 2
x * 3
```
Answer: 15

**10.** (Short Answer) A cell near the bottom of a notebook shows `[7]`, and the cell above it shows `[9]`. What does that tell you?

Answer: It means the cell below was executed earlier (execution count 7) than the cell above it (execution count 9). The execution order does not match the visual order of the cells.

**11.** (True/False) If you delete the cell that created the variable `message`, `message` is removed from memory immediately.

Answer: False

**12.** (Short Answer) Why should you **Restart & Run All** before submitting a notebook?
Answer: Restart & Run All checks that the notebook works from beginning to end in the correct order. It ensures that no cell depends on variables or results left over from previous executions.
---

## Section C — Getting Help (03)

**13.** (Short Answer) `?` shows a docstring. What does `??` sometimes show that `?` doesn't, and why doesn't it always work?

Answer: ?? sometimes displays the source code or implementation of an object, in addition to its documentation. It does not always work because some objects are built into Python or implemented in compiled code, so their source code may not be available.

**14.** (Multiple Choice) The signature is `sorted(iterable, /, *, key=None, reverse=False)`. Which call is valid?
A) `sorted(iterable=nums)`  B) `sorted(nums, None, True)`  C) `sorted(nums, reverse=True)`  D) `sorted()`

Answer: C

**15.** (Code Tracing) What error does this raise, and what does it mean?
```python
numbers = [1, 2, 3]
numbers.mean()
```
Answer: AttributeError

**16.** (Short Answer) A long traceback fills the screen. Which line should you read first, and why?

Answer: Read the last line of the traceback first because it contains the error type and message. Then look upward to find the line of code that caused the error.

**17.** (Short Answer) What does `dir(obj)` give you that Tab completion doesn't?

Answer: dir(obj) displays a list of the attributes and methods available for an object. Tab completion helps you discover these interactively while typing, but dir() gives you the full list at once.

---

## Section D — Markdown (04)

**18.** (Short Answer) What's the Markdown syntax for a level-2 heading (like `## Section 2`)?

Answer: 
## Section 2

**19.** (Short Answer) How do you write **inline** LaTeX math in a Markdown cell, versus a **block** equation on its own line(s)?

Answer: inline LaTeX uses one $ on each side: $x^2$ and block LaTeX uses two $$ on each side, usually on separate lines

**20.** (Short Answer) The Markdown text `The item costs $5 and the tax is $2.` renders strangely. Why, and how do you fix it?

Answer: The dollar signs can be interpreted as delimiters for LaTeX math, causing the text to render strangely

**21.** (Short Answer) You typed two lines directly under each other in a Markdown cell, but they render as **one** line. Name two ways to fix it.

Answer: Add two spaces at the end of the first line to create a line break or leave a blank line between the two lines to create separate paragraphs.

**22.** (Short Answer) Why might you add narrative Markdown cells between code cells instead of just writing code comments?
Answer: 
Narrative Markdown cells help explain the purpose of the code, describe results, and organize the notebook with headings and paragraphs. Unlike code comments, Markdown can create formatted explanations that are easier for readers to follow
---

## Section E — Files and Paths (05)

**23.** (Code Tracing) What does this print?
```python
with open('x.txt', 'w', encoding='utf-8') as f:
    f.write('hi')

with open('x.txt', 'r', encoding='utf-8') as f:
    print(f.read())
```

Answer: hi

**24.** (Multiple Choice) Which file mode adds to the end of an existing file **without** erasing it?
A) `'r'`  B) `'w'`  C) `'a'`  D) `'x'`

Answer: C

**25.** (Code Tracing) What does this print?
```python
from pathlib import Path
p = Path('data') / 'sales_2026.csv'
print(p.name, p.stem, p.suffix)
```
Answer: sales_2026.csv sales_2026 .csv

**26.** (Short Answer) Why is it good practice to use a relative path (like `'data/file.csv'`) instead of an absolute path (like `'C:/Users/yourname/...'`) in a notebook you'll share or submit?

Answer: Relative paths make notebooks easier to share because they do not depend on a specific computer's folder structure or username. Other people can run the notebook as long as the required files are in the expected relative locations.

**27.** (Code Tracing) `scores.csv` contains the two lines `Name,Score` and `Alice,90`. What does this print, and why might that cause a problem?
```python
import csv
with open('scores.csv', newline='', encoding='utf-8') as f:
    rows = list(csv.reader(f))
print(rows[1])
```

Answer: ['Alice', '90']

**28.** (True/False) After saving `{'point': (3, 4)}` with `json.dump` and reading it back with `json.load`, you get back exactly the same dictionary.

Answer: False
---

## Section F — Data Input and Output (06)

**29.** (Short Answer) What does `pd.read_csv('file.csv')` return?

Answer: pd.read_csv('file.csv') returns a Pandas DataFrame containing the data from the CSV file.

**30.** (Multiple Choice) Which DataFrame method shows summary statistics (mean, standard deviation, min/max, etc.)?
A) `.head()`  B) `.info()`  C) `.describe()`  D) `.columns()`

Answer: C

**31.** (Short Answer) You save a DataFrame with `df.to_csv('out.csv')`, read it back, and find an extra `Unnamed: 0` column. What caused it, and how do you prevent it?

Answer: The extra Unnamed: 0 column appears because Pandas saves the DataFrame index by default when using to_csv(). Prevent by using: df.to_csv('out.csv', index=False)

**32.** (Short Answer) What does JSON written with `orient='records'` look like, and why is it usually the layout you want?

Answer: JSON with orient='records' looks like a list of dictionaries, where each dictionary represents one row.

**33.** (Short Answer) After `pd.read_csv`, the ZIP code `02108` shows up as `2108`. Why, and how do you fix it?

Answer: df = pd.read_csv('file.csv', dtype={'ZIP': str})

**34.** (Short Answer) What does `pd.read_excel('file.xlsx', sheet_name=None)` return?

Answer: It returns a dictionary of DataFrames, where each dictionary key is a sheet name and each value is the DataFrame containing that sheet's data.

---

## Section G — Plotting with NumPy and Matplotlib (07)

**35.** (Code Tracing) What do these two lines print?
```python
print([1, 2] * 2)
print(np.array([1, 2]) * 2)
```
Answer: 
[1, 2, 1, 2]
[2 4]

**36.** (Matching) Choose the best chart (line, bar, scatter, or histogram) for each question:
a) Which department sold the most?
b) How did daily website visits change over a month?
c) Is there a relationship between price and units sold?
d) What is the distribution of customer order values?

Answer: 

a. bar chart
b. line chart
c. scatter plot
d. histogram

**37.** (Short Answer) A histogram and a bar chart both draw rectangles. What's the difference in what they show?
Answer: A histogram shows the distribution of numerical data by grouping values into intervals (bins). A bar chart compares separate categories, such as departments or products.

**38.** (Short Answer) You call `plt.show()` and then `plt.savefig('chart.png')`, and the saved image is blank. Why?

Answer: plt.show() displays the plot and may clear or close the figure, depending on the environment. Calling plt.savefig() afterward can therefore save a blank image.

**39.** (Short Answer) On a box plot, what does the line **inside** the box represent, and what do the dots **beyond** the whiskers represent?
Answer: The line inside the box represents the median (middle value) of the data. The dots beyond the whiskers represent potential outliers, which are unusually low or high values compared with the rest of the data.
---

## Section H — Timing and Performance (08)

**40.** (Short Answer) What's the difference between `%time` and `%timeit`, and why is `%timeit` more trustworthy for comparisons?

Answer: - %time measures how long one execution takes while %timeit runs the code multiple times and calculates timing statistics. %timeit is generally more trustworthy because repeated measurements reduce the effect of random variations and background computer activity.

**41.** (Short Answer) Why is a NumPy vectorized operation like `arr ** 2` usually much faster than a Python `for` loop doing the same calculation element by element?

Answer: NumPy vectorized operations are usually faster because they perform calculations using optimized code implemented in lower-level languages. A Python for loop has additional overhead because Python processes each element individually.

**42.** (True/False) A variable created inside a `%%timeit` cell is available in the cells after it.

Answer: False

**43.** (Short Answer) What's unfair about comparing `%timeit np.array(my_list) ** 2` against `%timeit arr ** 2` to decide whether squaring is fast?

Answer: The comparison is unfair because np.array(my_list) includes the time needed to convert the list into an array, while arr ** 2 only measures the squaring operation. To make the comparison fair, create the NumPy array before timing both operations.

**44.** (Short Answer) Why is `[s / max(scores) for s in scores]` slow for large lists? How would you fix it, and roughly what happens to its runtime when the list doubles in size?

Answer: It's slow because max(scores) is recalculated for every element in the list. Calculate the maximum once before the loop: maximum = max(scores)
normalized = [s / maximum for s in scores]

**45.** (Multiple Choice) What does `%prun` show you?
A) Only the total time of the cell  B) How much time was spent in each function called  C) How much memory each variable uses  D) The source code of a function

Answer: B

