# 📝 Worksheet: 06 - Working with Data (Advanced)

Use this worksheet to reinforce Pandas Series/DataFrames and the core data-wrangling workflow: load, explore, select, clean, transform, sort/filter/search, group, and combine. Each section ends with an **🤖 Explain It** prompt — write your explanation in your own words, then (optionally) paste it into an AI tutor and ask it to point out anything you got wrong or left out.

---

## 🐼 Section 1: Series and DataFrames

1. What's the difference between a Pandas `Series` and a `DataFrame`? How does a list of dictionaries (from Module 03) turn into one?  
   `Answer:` ___A Series is a one-dimensional structure, similar to one column of data. A DataFrame is a two-dimensional structure, similar to a table with rows and columns. When we use pd.DataFrame() with a list of dictionaries, each dictionary becomes a row, and the dictionary keys become the column names

2. If you add two `Series` together and a label appears in only one of them, what value do you get at that label, and why?  
   `Answer:` ____The result will be NaN because Pandas matches values using their index labels. If a label is missing from one Series, Pandas cannot calculate the sum for that label.__

---

### ✏️ Task: Series Basics

```python
# Create a Series from a dictionary mapping 4 of your classes to your grade in each.
# Print the Series, then print the average of all grades (hint: .mean()).

import pandas as pd

# Create a Series from a dictionary
grades = pd.Series({
    'Computer Science': 95,
    'Mathematics': 92,
    'English': 88,
    'History': 90
})

# Print the Series
print(grades)

# Calculate and print the average
print("\nAverage grade:", grades.mean())
```

### 🤖 Explain It

In your own words: how is a Pandas `Series` similar to a Python dictionary, and how is it similar to a list? What does it borrow from each?

- A Pandas Series is similar to a Python dictionary because both store values with labels or keys that we can use to access specific information. It is also similar to a list because it stores multiple values in an ordered structure. A Series combines ideas from both: it stores multiple values like a list, but each value can also have an index label like a dictionary.

---

## 📂 Section 2: Loading and Exploring

3. What does `df.info()` tell you that `df.describe()` doesn't?  
   `Answer:` _.info() tells us the column names, data types, number of non-missing values, and memory usage. .describe() provides summary statistics such as the mean, standard deviation, minimum, and maximum.
4. If `df.isnull().sum()` shows `Age: 177`, what does that number mean?  
   `Answer:` __It means that the Age column contains 177 missing values.___

5. In `titanic.csv`, what does **one row** represent, and which column identifies each row?  
   `Answer:` ___Each row represents one passenger on the Titanic. The PassengerId column identifies each passenger with a unique ID_

---

### ✏️ Task: First Look at a Dataset

```python
# Load pokemon.csv.
# Print its shape, its columns, and .describe() for just the numeric columns.
# Which column has the most missing values, if any?

# Load pokemon.csv
pokemon = pd.read_csv('pokemon.csv')

# Print dataset dimensions
print("Shape:", pokemon.shape)

# Print column names
print("\nColumns:")
print(pokemon.columns.tolist())

# Describe numeric columns
print("\nNumeric column statistics:")
print(pokemon.describe())

# Find missing values
print("\nMissing values:")
print(pokemon.isnull().sum())

# Find the column with the most missing values
missing_counts = pokemon.isnull().sum()

print("\nColumn with the most missing values:")
print(missing_counts.idxmax())

print("Number of missing values:", missing_counts.max())
```

### 🤖 Explain It

In your own words: why is it worth running `.info()` and `.isnull().sum()` on a dataset *before* you start analyzing it, rather than jumping straight to the questions you actually care about?

- It is important to run .info() and .isnull().sum() before analyzing a dataset because they help us understand the structure and quality of the data. We can identify missing values, incorrect data types, and columns that might need cleaning. If we start analyzing without checking these things, our results could be incorrect or misleading
---

## 🎯 Section 3: Indexing and Selection

6. Rewrite this using `.loc[]` instead of bracket + `.iloc[]` mixed together — or explain why you can't mix them the way it's written:
```python
df.iloc[df['Age'] < 10]  # this is actually broken — why?
```
   `Answer:` ____This is incorrect because .iloc[] expects integer positions, but df['Age'] < 10 produces a Boolean Series containing True and False values._

   - df.loc[df['Age'] < 10]

7. What's wrong with `df[df['Age'] < 10 and df['Fare'] > 100]`, and how would you fix it?  
   `Answer:` ___The problem is that Python's and keyword cannot combine Pandas Series conditions. We need to use & instead, with parentheses around each condition.___

   - df[(df['Age'] < 10) & (df['Fare'] > 100)]

8. Rewrite `df[(df['Pclass'] == 1) | (df['Pclass'] == 2)]` with `.isin()`. Why is `.isin()` easier to read when there are five values to match instead of two?  
   `Answer:` ___df[df['Pclass'].isin([1, 2])]___

---

### ✏️ Task: Titanic Slicing

```python
# Using titanic.csv:
# 1. Select passengers in 3rd class (Pclass == 3) who survived (Survived == 1).
# 2. Of those, show only the Name, Age, and Fare columns.
# 3. Sort the result by Fare, descending.

# Load Titanic dataset
titanic = pd.read_csv('titanic.csv')

# Select passengers in 3rd class who survived
survivors = titanic.loc[
    (titanic['Pclass'] == 3) & (titanic['Survived'] == 1)
]

# Select only Name, Age, and Fare
survivors = survivors[['Name', 'Age', 'Fare']]

# Sort by Fare from highest to lowest
survivors = survivors.sort_values('Fare', ascending=False)

# Display results
survivors
```

### 🤖 Explain It

In your own words: why does `df.iloc[df['Age'] < 10]` not work, when `df.loc[df['Age'] < 10]` and `df[df['Age'] < 10]` both do?

- .iloc[] uses integer positions, such as 0, 1, 2, and 3. However, df['Age'] < 10 produces a Boolean condition rather than integer positions. .loc[] works because it accepts Boolean conditions and selects the rows where the condition is True. Regular bracket filtering works for the same reason.

---

## 🧼 Section 4: Cleaning

9. If a column has the values `['25', 'thirty', '35']`, what happens when you run `pd.to_numeric(col, errors='coerce')` on it?  
   `Answer:` __Pandas converts '25' and '35' into numbers, but 'thirty' cannot be converted. Because we use errors='coerce', Pandas replaces 'thirty' with NaN
10. When would you choose `.dropna()` over `.fillna()`, and vice versa?  
   `Answer:` _I would use .dropna() when rows or columns with missing values are not useful for my analysis. I would use .fillna() when I want to keep the data and replace missing values with something appropriate, such as an average, zero, or a placeholder.__

11. A survey stores missing ages as `-1`. Why is that *worse* for analysis than a blank (`NaN`), and what one-line fix turns them into real missing values?  
   `Answer:` _It is worse because Pandas treats -1 as a real number, so it includes it when calculating averages and other statistics. We can replace it with a real missing value: df['age'] = df['age'].replace(-1, pd.NA)__

12. Two rows share an `order_id` but have different amounts. Will `df.drop_duplicates()` remove one? What argument makes it compare only `order_id`?  
   `Answer:` ___No. By default, .drop_duplicates() compares all columns, so rows with different amounts are not considered exact duplicates.df.We can specify subset to compare only the order_id column: drop_duplicates(subset=['order_id'])____

---

### ✏️ Task: Clean a Column

```python
# Using ufo.csv:
# 1. Check .isnull().sum() for the whole dataset.
# 2. Pick the column with the most missing values.
# 3. Decide (and justify in a comment) whether dropping or filling makes more
#    sense for that specific column, then do it.

# Load UFO dataset
ufo = pd.read_csv('ufo.csv')

# Check missing values in the entire dataset
print("Missing values:")
print(ufo.isnull().sum())

# Find the column with the most missing values
missing_counts = ufo.isnull().sum()

column_with_most_missing = missing_counts.idxmax()
number_missing = missing_counts.max()

print("\nColumn with most missing values:", column_with_most_missing)
print("Number of missing values:", number_missing)

# Decide how to handle the missing values
# We will drop the column if most of its values are missing,
# because it may not provide enough useful information.

if number_missing > 0:
    if number_missing / len(ufo) > 0.5:
        # More than half the values are missing, so drop the column.
        ufo = ufo.drop(columns=[column_with_most_missing])
        print("\nDropped column:", column_with_most_missing)
    else:
        # Keep the column because most values are still available.
        # Missing categorical values are marked as Unknown.
        ufo[column_with_most_missing] = (
            ufo[column_with_most_missing].fillna('Unknown')
        )
        print("\nFilled missing values in:", column_with_most_missing)
else:
    print("\nNo missing values found.")
```

### 🤖 Explain It

In your own words: why might dropping rows with missing data be a bad idea if a large fraction of your dataset has at least one missing value somewhere?
- Dropping rows with missing data can be a bad idea if a large percentage of the dataset has missing values because we could lose too much useful information. It can also make the remaining data less representative of the original dataset. Sometimes it is better to fill missing values with an appropriate value instead of deleting entire rows.
---

## 🧱 Section 5: Column Operations, Sorting, and Filtering

13. What's the difference between `df.rename(columns={'a': 'b'})` and `df.columns = ['b', 'c', 'd']`?  
   `Answer:` ___.rename() changes only the column names that we specify, leaving the other names unchanged. Assigning a new list to df.columns replaces all column names at once, so we must provide the correct number of names__

14. Rewrite this bracket-filter as a `.query()` call:
```python
df[(df['math_score'] >= 80) & (df['science_score'] >= 80)]
```
   `Answer:` __df.query('math_score >= 80 and science_score >= 80')__

15. If `status` is a brand-new column, what do the rows with `score >= 70` contain after `df.loc[df['score'] < 70, 'status'] = 'review'`? How do you give every row a default first?  
   `Answer:` __The rows with score >= 70 will contain missing values (NaN) because we only assigned 'review' to rows with scores below 70. We can assign a default value first: 
   
   df['status'] = 'good' 
   
   df.loc[df['score'] < 70, 'status'] = 'review'_

---

### ✏️ Task: Build a Feature

```python
# Using students.csv:
# 1. Create a "total_score" column (math + science).
# 2. Sort by total_score, descending.
# 3. Print just the top 3 students' first_name and total_score.

# Load students dataset
students = pd.read_csv('students.csv')

# Create total_score
students['total_score'] = students['math'] + students['science']

# Sort from highest to lowest
students = students.sort_values('total_score', ascending=False)

# Display the top 3 students
students[['first_name', 'total_score']].head(3)
```

### ✏️ Task: Loop vs. Vectorized

```python
# Using students.csv:
# 1. Build a list of each student's total score with an .itertuples() loop.
# 2. Compute the same totals with one whole-column expression.
# 3. Confirm they match, then use %timeit to compare the two.

# 1. Calculate total scores using an itertuples() loop
loop_totals = []

for row in students.itertuples(index=False):
    loop_totals.append(row.math + row.science)

print("Loop totals:")
print(loop_totals)

# 2. Calculate total scores using a vectorized expression
vectorized_totals = (students['math'] + students['science']).tolist()

print("\nVectorized totals:")
print(vectorized_totals)

# 3. Confirm that they match
print("\nDo they match?", loop_totals == vectorized_totals)

# 4. Compare execution times
print("\nLoop time:")
%timeit [row.math + row.science for row in students.itertuples(index=False)]

print("\nVectorized time:")
%timeit students['math'] + students['science']
```

### 🤖 Explain It

In your own words: what does "feature engineering" mean in the context of `total_score` or `average_score` — why create a new column instead of just computing the value fresh every time you need it?

- Feature engineering means creating new columns from existing data to make analysis easier and more useful. For example, we can create total_score by adding math and science scores, or average_score by calculating their average. Creating these columns makes the information easier to reuse, compare, sort, and analyze without repeating the same calculation every time.
---

## 🧩 Section 6: Grouping and Combining

16. Describe the three steps of `df.groupby('Pclass')['Survived'].mean()` (split, apply, combine) in plain words. What does the result tell you?  
   `Answer:` 
   1. Split: Pandas separates passengers into groups based on their ticket class (Pclass).
   2. Apply: It calculates the average survival value for each group.
   3. Combine: It puts the results together into a Series showing the survival rate for each class.

17. What's the difference between `pd.concat()` and `.merge()`, and between a `'left'` and an `'inner'` merge?  
   `Answer:` __pd.concat() combines DataFrames by stacking them vertically or horizontally. .merge() combines DataFrames using a shared column or key.
- A left merge keeps every row from the left DataFrame, even when there is no match in the right DataFrame.
- An inner merge keeps only rows that have matching keys in both DataFrames.__

---

### ✏️ Task: Survival by Group

```python
# Using titanic.csv:
# 1. Find the survival rate by Pclass, then by Pclass AND Sex together.
# 2. Use .agg() to show, per Embarked port: passengers, average Fare, survival rate.
# 3. .reset_index() the result and sort it by survival rate, highest first.

# Load Titanic dataset
titanic = pd.read_csv('titanic.csv')

# 1. Survival rate by Pclass
survival_by_class = (
    titanic
    .groupby('Pclass')['Survived']
    .mean()
)

print("Survival rate by class:")
print(survival_by_class)

# Survival rate by Pclass AND Sex
survival_by_class_sex = (
    titanic
    .groupby(['Pclass', 'Sex'])['Survived']
    .mean()
)

print("\nSurvival rate by class and sex:")
print(survival_by_class_sex)

# 2. Statistics by Embarked port
port_summary = (
    titanic
    .groupby('Embarked')
    .agg(
        passengers=('PassengerId', 'count'),
        average_fare=('Fare', 'mean'),
        survival_rate=('Survived', 'mean')
    )
    .reset_index()
)

# 3. Sort by survival rate, highest first
port_summary = port_summary.sort_values(
    'survival_rate',
    ascending=False
)

print("\nSummary by embarkation port:")
port_summary
```

### ✏️ Task: Join Two Tables

```python
# Build a table of 5 students (student_id, name) and a table of 4 grades
# (student_id, grade), where one student has no grade.
# Merge them with how='left' and with how='inner'. How many rows does each give, and why?

# Create a table of 5 students
students = pd.DataFrame({
    'student_id': [1, 2, 3, 4, 5],
    'name': ['Ana', 'Ben', 'Carlos', 'Diana', 'Emma']
})

# Create a table of 4 grades
grades = pd.DataFrame({
    'student_id': [1, 2, 3, 4],
    'grade': [95, 88, 91, 85]
})

# Left merge
left_result = students.merge(
    grades,
    on='student_id',
    how='left'
)

print("Left merge:")
print(left_result)
print("Number of rows:", len(left_result))

# Inner merge
inner_result = students.merge(
    grades,
    on='student_id',
    how='inner'
)

print("\nInner merge:")
print(inner_result)
print("Number of rows:", len(inner_result))
```

### 🤖 Explain It

In your own words: when you merge UFO sightings (state codes like `'tx'`) with populations (codes like `'TX'`), every state fails to match at first. Why, and what does that teach you about checking keys *before* you join?
- The merge fails because Pandas compares text values exactly, and 'tx' and 'TX' are different strings. We should standardize the state codes before joining the tables, for example by converting them all to uppercase using .str.upper(). This teaches us that we must check and clean the columns used as keys before merging tables, otherwise valid records might not match
---

## 🧾 Submit Checklist

- [x] I created a `Series` and a `DataFrame` from Python containers (a dictionary or a list of dictionaries).
- [x] I loaded a local CSV into a DataFrame and inspected it with `.info()` and `.describe()`.
- [x] I used both `.loc[]` and `.iloc[]` at least once each.
- [x] I filtered a DataFrame using a compound condition (`&` or `|`).
- [x] I cleaned at least one column with `.fillna()`, `.dropna()`, or `pd.to_numeric()`.
- [x] I created a new column and sorted by it.
- [x] I removed duplicate rows or replaced a placeholder value with `NaN`.
- [x] I filtered with `.isin()`, `.between()`, or `.str.contains()`.
- [x] I used a whole-column (vectorized) calculation instead of a row-by-row loop.
- [x] I summarized groups with `groupby()` (and `.agg()`), and combined tables with `pd.concat()` or `.merge()`.
- [x] I completed the "Explain It" prompts in my own words.
