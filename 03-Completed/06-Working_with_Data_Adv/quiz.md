# 📝 Quiz: 06 - Working with Data (Advanced)

47 questions covering Pandas Series/DataFrames, loading/exploring data, indexing/selection, cleaning, column operations, sorting/filtering/searching, grouping and combining tables, and a "going further" bonus. Mix of multiple choice, true/false, code-tracing, and short answer.

Try every question on your own first — the Answer Key is at the bottom, no peeking. If you're using an AI tutor, paste in *your answer* and ask it to check your reasoning rather than asking it to solve the question first.

---

## Section A — Series and DataFrames

**1.** (Short Answer) What's the difference between a Pandas `Series` and a `DataFrame`?

Answer: 
- A Series is a one-dimensional structure, like a single column.
- A DataFrame is a two-dimensional structure, like a table with rows and columns.

**2.** (Code Tracing) What prints?
```python
s = pd.Series({'a': 1, 'b': 2})
print(s['a'])
```
Answer: 1

**3.** (Short Answer) What does `df.shape` return?
Answer: It returns a tuple containing the number of rows and columns (rows, columns)

**4.** (Short Answer) If you add two `Series` together and one has a label the other doesn't, what happens at that label?

Answer: Pandas aligns values using their index labels. If a label is missing from either Series, the result at that label is NaN

**5.** (Short Answer) You have a list of dictionaries, one per student (the way you stored records in Module 03b). What does `pd.DataFrame(that_list)` turn each dictionary into?

 Answer: Each dictionary becomes a row, and its keys become column names. The dictionary values become the corresponding cell values

**6.** (Code Tracing) What prints?
```python
scores = pd.Series([70, 95, 88])
print(scores[scores > 80].tolist())
```
Answer: [95 88]
---

## Section B — Loading and Exploring Data

**7.** (Multiple Choice) Which method shows summary statistics (mean, std, min/max) for numeric columns?
A) `.head()`  B) `.columns`  C) `.describe()`  D) `.shape`

Answer: C

**8.** (Short Answer) What's the difference between what `.head()` and `.info()` each tell you about a DataFrame?

Answer: 
- .head() displays the first five rows by default so we can see the actual data.

- .info() displays structural information, including column names, data types, non-null counts, and memory usage.

**9.** (Short Answer) How do you count how many missing values are in *each column* of a DataFrame?

Answer: .isna() identifies missing values, and .sum() counts them in each column

**10.** (Short Answer) What do `df.duplicated().sum()` and `df['col'].value_counts(dropna=False)` each tell you?

Answer: 
- df.duplicated().sum() counts the number of duplicate rows.
- df['col'].value_counts(dropna=False) counts how many times each unique value appears in a column, including missing values.

**11.** (Short Answer) Titanic's `Age` column holds whole numbers, but its dtype is `float64`. Why?

Answer: Because the column contains missing values (NaN). Regular integer types cannot represent NaN, so Pandas often uses floating-point numbers instead
---

## Section C — Indexing and Selection

**12.** (Short Answer) What's the difference between `.loc[]` and `.iloc[]`?

Answer: 
- .loc[] selects using row or column labels.
- .iloc[] selects using integer positions.

**13.** (Short Answer) Is `df.loc[0:4]` inclusive or exclusive of row `4`? What about `df.iloc[0:4]` and position `4`?

Answer:

- .loc[0:4] includes index label 4.
- .iloc[0:4] excludes position 4, selecting positions 0, 1, 2, and 3.

**14.** (Short Answer) What symbols does Pandas require for combining conditions when filtering a DataFrame (instead of the `and`/`or` keywords)?

Answer: & means AND, | means OR, ~ means NOT.

**15.** (Short Answer) What does `df[df['Age'] < 10]` return?

Answer: It returns a DataFrame containing only rows where the Age value is less than 10

**16.** (Short Answer) Rewrite `df[(df['Embarked'] == 'C') | (df['Embarked'] == 'Q')]` using `.isin()`. Does `df['Age'].between(13, 19)` include ages 13 and 19?

ANswer: df[df['Embarked'].isin(['C', 'Q'])]

**17.** (Multiple Choice) Which one returns a `DataFrame` rather than a `Series`?
A) `df['Age']`  B) `df[['Age']]`  C) `df.Age`  D) `df.loc[0]`

Answer: B

**18.** (Short Answer) After `by_id = df.set_index('PassengerId')`, what does `by_id.loc[2]` select, and how is that different from `df.loc[2]`?

Answer: 
- by_id.loc[2] selects the passenger whose PassengerId is 2.
- df.loc[2] selects the row whose original index label is 2.

**19.** (Code Tracing) After `by_id = titanic.set_index('PassengerId')`, do `by_id.loc[3]` and `by_id.iloc[3]` select the same passenger? Why or why not?
Answer: 
No because:
- by_id.loc[3] selects the passenger with PassengerId equal to 3.
- - by_id.iloc[3] selects the fourth row by position.

**20.** (Short Answer) Write a filter for passengers who did **not** board at Southampton (`'S'`), using `~` and `.isin()`.

Answer: df[~df['Embarked'].isin(['S'])]

The ~ symbol reverses the condition, selecting passengers whose embarkation value is not 'S'. This also includes missing embarkation values.

**21.** (Short Answer) What's wrong with `df[df['Age'] < 10]['child'] = True`, and what should you write instead?

Answer: It uses chained indexing, which may modify a temporary filtered copy rather than the original DataFrame. 

- df.loc[df['Age'] < 10, 'child'] = True

---

## Section D — Cleaning

**22.** (Short Answer) What's the difference between `.dropna()` and `.fillna()`?

- .dropna() removes rows or columns containing missing values.
- .fillna() replaces missing values with something else, such as a number, text, or calculated value.


**23.** (Short Answer) What does `pd.to_numeric(col, errors='coerce')` do to a value like `'ninety'` in that column?

Answer: It attempts to convert the value into a number. Since 'ninety' cannot be converted, Pandas replaces it with NaN.

**24.** (True/False) `NaN` and an empty string `""` are always treated identically by `.isnull()`.

Answer: False

**25.** (Short Answer) Two rows have the same `order_id` but different `amount`s. Does `df.drop_duplicates()` remove one of them? What would you write to keep only the last row for each `order_id`?

Answer: No. By default, drop_duplicates() compares all columns. If the amounts differ, the rows are not exact duplicates.
- df = df.drop_duplicates(subset=['order_id'], keep='last')
**26.** (Short Answer) A survey stores missing ages as `-1`. Why is that a problem for `df['age'].mean()`, and how do you fix it?

Answer: Because Pandas treats -1 as a real number, not as missing data. It includes -1 when calculating the average, making the mean incorrect.
- df['age'] = df['age'].replace(-1, pd.NA)

**27.** (Short Answer) A date column holds `'2022-01-01'`, `'2022/03/01'`, and `'April 5, 2022'`. Why can `pd.to_datetime(col, errors='coerce')` turn two perfectly valid dates into `NaT`, and how do you fix it?

Answer: Pandas may infer one date format and then fail to parse values written in other formats.
```python
df['date'] = pd.to_datetime(
    df['date'],
    errors='coerce',
    format='mixed'
)
```


**28.** (Short Answer) Why does `.astype('int64')` fail on a column with missing values, while `.astype('Int64')` works?

Answer: 

- int64 is a regular integer type and cannot store missing values.
- Int64 is Pandas' nullable integer type, which supports missing values (<NA>).

**29.** (Short Answer) Before deleting rows with a repeated `order_id`, why would you look at `df[df.duplicated(subset=['order_id'], keep=False)]` first?
Answer: To inspect all rows with repeated order_id values before deleting anything.

This helps us determine whether they are exact duplicates or different records, such as corrections or updated orders. We should understand the data before deciding which row to keep.
---

## Section E — Column Operations, Sorting, Filtering, and Searching

**30.** (Short Answer) How do you create a new column that's the sum of two existing numeric columns?

- df['total'] = df['price'] + df['tax']

**31.** (Short Answer) What does `.apply()` do that you could also do with a `for` loop over rows — and why is `.apply()` usually preferred?

Answer: .apply() applies a function to values in a Series or along an axis of a DataFrame.

It is usually shorter and easier to read than manually looping through rows. However, for simple column calculations, vectorized operations are generally even better than .apply().

**32.** (Short Answer) What does `ascending=False` do inside `df.sort_values(by='score', ascending=False)`?

Answer: It sorts the scores from largest to smallest. 

**33.** (Short Answer) What's the main advantage of `.query()` over bracket-based boolean filtering (`df[df['col'] > 5]`)?

Answer: .query() can make complicated filters easier to read because its syntax resembles a normal sentence.

**34.** (Code Tracing) `bonus` is a brand-new column. What value do the rows with `score >= 80` get in it?
```python
df.loc[df['score'] < 80, 'bonus'] = 5
```

Answewr: NaN

**35.** (Short Answer) To compute `price * qty` for every row, why should you write `df['price'] * df['qty']` instead of looping with `.iterrows()`? When *is* a loop (preferably `.itertuples()`) the right tool?

Answer: This is a vectorized operation, meaning Pandas performs the calculation across the entire column efficiently.

A loop, preferably .itertuples(), is useful when each row requires a separate action or complicated logic that cannot be easily expressed using vectorized operations.

**36.** (Short Answer) What do `case=False` and `na=False` each do in `df['name'].str.contains('an', case=False, na=False)`?

Answer: 
- case=False: ignores capitalization, so 'AN', 'an', and 'An' all match.
- na=False: treats missing values as False instead of returning missing results.

**37.** (Code Tracing) A student has `math = 95` and `average = 91`. What is their `award` after these three lines, and what would it be if the last two lines were swapped?
```python
df['award'] = 'None'
df.loc[df['average'] >= 85, 'award'] = 'Honor Roll'
df.loc[df['math'] >= 90, 'award'] = 'Math Star'
```

ANswer: Math Star

**38.** (Short Answer) What does `pd.cut(df['score'], bins=[0, 70, 90, 101], labels=['Low', 'Mid', 'High'], right=False)` do, and why is it usually better than `.apply()` with an `if`/`elif` function for the same job?

Answer: It divides numeric scores into categories (Low, Mid and High), right=False means the right endpoint is excluded from each interval, .cut() is usually more convenient and efficient for dividing an entire column into ranges than writing a separate function with multiple if/elif statements.

**39.** (Short Answer) What's the difference between `df.query('math_score >= minimum')` and `df.query('math_score >= @minimum')`?

Answer: 
- Without @, Pandas looks for a column or variable named minimum inside the query environment.
- With @, Pandas knows to use the Python variable named minimum defined outside the query.

**40.** (Code Tracing) What does each line return, and why are they different?
```python
books = pd.Series(['C++ Primer', 'C Programming'])
books.str.contains('C++').tolist()
books.str.contains('C++', regex=False).tolist()
```
Answer: 
- [True, True]
- [True, False]
---

## Section F — Grouping and Combining

**41.** (Code Tracing) What does this return?
```python
df = pd.DataFrame({'team': ['A', 'B', 'A', 'B'], 'points': [10, 4, 20, 6]})
df.groupby('team')['points'].mean()
```

ANswer:

team
A    15.0
B     5.0
Name: points, dtype: float64

**42.** (Short Answer) Write a `groupby` with `.agg()` that gives, for each `team`, the number of rows and the total `points`, as columns named `games` and `total`.

Answer:
```python
df.groupby('team').agg(
    games=('points', 'count'),
    total=('points', 'sum')
)
```

**43.** (Short Answer) When would you use `pd.concat()` instead of `.merge()`? What does `ignore_index=True` do?

- pd.concat() combines DataFrames by stacking rows or columns. Use it when combining similar datasets, such as January and February sales.
- .merge() combines tables using a shared key, such as buyer_id.
- ignore_index=True creates a new sequential index instead of keeping the original row indexes.

**44.** (Short Answer) In `orders.merge(prices, on='product_id', how='left')`, what happens to an order whose `product_id` isn't in `prices`? What would `how='inner'` do instead? And why does a merge on state codes fail when one table uses `'tx'` and the other uses `'TX'`?
- With orders.merge(prices, on='product_id', how='left') The order remains in the result, but the price information is missing (NaN) because no matching product was found.  State codes like 'tx' and 'TX' do not match because string comparisons are case-sensitive.


**45.** (Short Answer) In Titanic, `groupby('Pclass').size()` gives 216 for 1st class, but `groupby('Pclass')['Age'].count()` gives 186. Why are they different?
- .size() counts every row in the group, including rows with missing ages.
- .count() counts only non-missing values in the selected column (Age). Therefore, 30 first-class passengers have missing ages.

**46.** (Short Answer) `purchases` has 6 rows. After `purchases.merge(buyers, on='buyer_id', how='left')`, it has 8. What probably happened, and how can `validate=` protect you?

Answer: 
The buyers table probably has duplicate buyer_id values. One purchase may match multiple buyer rows, causing the merge to create additional rows. validate='many_to_one' checks that each purchase matches at most one buyer record. If the buyer IDs are not unique, Pandas raises an error instead of silently producing duplicated purchase rows.

```python
purchases.merge(
    buyers,
    on='buyer_id',
    how='left',
    validate='many_to_one'
)
```
---

## Section G — Going Further (Bonus)

**47.** (Short Answer) What do the `usecols` and `nrows` arguments to `pd.read_csv()` do, and when would you reach for them?

- usecols lets you load only specific columns from a CSV file.

- nrows lets you load only a certain number of rows.

- I would use these arguments when working with large CSV files and only needing a small part of the data. They can save memory and make loading faster. 