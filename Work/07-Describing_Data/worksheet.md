# 📝 Worksheet: 07 - Describing Data

Use this worksheet to reinforce the descriptive workflow: what each column means, what's typical, how much it varies, what's unusual, how groups differ, and which columns move together. Each section ends with an **🤖 Explain It** prompt. Write your explanation in your own words, then (optionally) paste it into an AI tutor and ask it to point out anything you got wrong or left out.

---

## 🏷️ Section 1: Types of Data

1. Classify each as nominal, ordinal, interval, or ratio: `zip code`, `t-shirt size (S/M/L/XL)`, `year of birth`, `monthly rent`.  
   `Answer:` __Zip Code = Nominal, T-short size = Ordinal, Year of birth = Interval, monthly rent = ratio____

2. A column has dtype `int64` and only 4 distinct values. Name two things it might really be, and how you'd decide.  
   `Answer:`

   - Nominal: For example, codes representing different countries or categories.
   - Ordinal: For example, ratings from 1 to 4, where the numbers represent an order.

---

### ✏️ Task: Record the Meaning

```python
# Using titanic.csv:
# 1. Convert Embarked to 'category' and map C/Q/S to 'Cherbourg'/'Queenstown'/'Southampton'.
# 2. Create an ordered 'class' column (3rd < 2nd < 1st).
# 3. Print the share of passengers above 3rd class, using a comparison on 'class'.


import pandas as pd
import numpy as np

titanic = pd.read_csv('titanic.csv')

# 1. Convert Embarked to readable names
titanic['Embarked'] = titanic['Embarked'].map({
    'C': 'Cherbourg',
    'Q': 'Queenstown',
    'S': 'Southampton'
})

titanic['Embarked'] = titanic['Embarked'].astype('category')

# 2. Create an ordered class column
titanic['class'] = pd.Categorical(
    titanic['Pclass'].map({
        3: '3rd',
        2: '2nd',
        1: '1st'
    }),
    categories=['3rd', '2nd', '1st'],
    ordered=True
)

# 3. Calculate the share of passengers above 3rd class
above_3rd = (titanic['class'] > '3rd').mean()

print('Share above 3rd class:', round(above_3rd, 3))
print('Percentage:', round(above_3rd * 100, 2), '%')
```

### 🤖 Explain It

In your own words: why does it matter whether a variable is nominal or ordinal before you compute anything with it? What could go wrong if you averaged an ordinal column's codes?


- It matters whether a variable is nominal or ordinal because it determines which calculations make sense. Nominal categories have no meaningful order, while ordinal categories do. If we average ordinal codes, the result might be misleading because the numerical differences between categories are not necessarily equal. For example, averaging 1st, 2nd, and 3rd class does not necessarily describe a meaningful average passenger class
---

## 🎯 Section 2: Center and Spread

3. Why is the median usually reported for house prices instead of the mean?  
   `Answer:` _The median is usually better because house prices can have extremely expensive properties that increase the mean. The median is less affected by these extreme values and better represents the typical house price___

4. You compute a standard deviation with pandas and get 14.53, and a classmate uses NumPy and gets 14.52. Who's wrong?  
   `Answer:` __Neither is necessarily wrong. Pandas uses ddof=1 by default, calculating sample standard deviation, while NumPy uses ddof=0, calculating population standard deviation by default__

---

### ✏️ Task: Compare Center and Spread

```python
# Using pokemon.csv:
# For Attack, print the mean, median, std, and IQR.
# Then do the same for Legendary Pokémon only.
# Which group is more spread out, by std and by IQR? Do the two measures agree?

pokemon = pd.read_csv('pokemon.csv')

# Calculate Attack statistics for all Pokémon
attack_all = pokemon['Attack']

print('All Pokémon:')
print('Mean:', attack_all.mean())
print('Median:', attack_all.median())
print('Standard deviation:', attack_all.std())
print('IQR:', attack_all.quantile(0.75) - attack_all.quantile(0.25))


# Calculate Attack statistics for Legendary Pokémon
attack_legendary = pokemon[pokemon['Legendary'] == True]['Attack']

print('\nLegendary Pokémon:')
print('Mean:', attack_legendary.mean())
print('Median:', attack_legendary.median())
print('Standard deviation:', attack_legendary.std())
print('IQR:', attack_legendary.quantile(0.75) - attack_legendary.quantile(0.25))
```


```python

# Create a comparison table
attack_comparison = pokemon.groupby('Legendary')['Attack'].agg(
    mean='mean',
    median='median',
    std='std',
    Q1=lambda x: x.quantile(0.25),
    Q3=lambda x: x.quantile(0.75)
)

attack_comparison['IQR'] = (
    attack_comparison['Q3'] - attack_comparison['Q1']
)

attack_comparison
```

### 🤖 Explain It

In your own words: two classes have the same average exam score, but very different standard deviations. Describe what being a student in each class might be like.

- If two classes have the same average exam score but different standard deviations, students in one class might have very similar scores, while students in the other class have scores that are more spread out. A smaller standard deviation means students performed more consistently. A larger standard deviation means there were bigger differences between students' scores.

---

## 📐 Section 3: Shape and Outliers

5. A column has mean 50, median 38, and `.skew()` of 2.1. Describe its shape.  
   `Answer:` _The column is strongly right-skewed because its mean is larger than its median and its skewness is positive. This suggests that some unusually large values are pulling the mean upward_

6. Name one situation where you **should** remove an outlier, and one where you shouldn't.  
   `Answer:`
   - Should remove: When an outlier is caused by a recording mistake, such as entering 3000 kg instead of 300 kg.
   - Shouldn't remove: When the value is real but unusual, such as a passenger paying an unusually expensive Titanic fare.
---

### ✏️ Task: Outlier Report

```python
# Using penguins.csv (drop the 2 rows with no measurements first):
# 1. Write or copy iqr_fences(series, k=1.5).
# 2. Count outliers in body_mass_g overall, then within each species.
# 3. Print the flagged rows. Are any of them impossible values?

penguins = pd.read_csv('penguins.csv')

MEASUREMENTS = [
    'bill_length_mm',
    'bill_depth_mm',
    'flipper_length_mm',
    'body_mass_g'
]

# Remove the two rows with no measurements
clean = penguins.dropna(subset=MEASUREMENTS).copy()

# IQR fence function
def iqr_fences(series, k=1.5):
    q1 = series.quantile(0.25)
    q3 = series.quantile(0.75)

    iqr = q3 - q1

    lower = q1 - k * iqr
    upper = q3 + k * iqr

    return lower, upper
```

```python
# 1. Outliers in body mass overall
low, high = iqr_fences(clean['body_mass_g'])

overall_outliers = clean[
    (clean['body_mass_g'] < low) |
    (clean['body_mass_g'] > high)
]

print('Overall body mass fences:', low, 'to', high)
print('Overall outliers:', len(overall_outliers))
```

```python
# 2. Calculate fences separately within each species
q1 = clean.groupby('species', observed=True)['body_mass_g'].transform(
    lambda x: x.quantile(0.25)
)

q3 = clean.groupby('species', observed=True)['body_mass_g'].transform(
    lambda x: x.quantile(0.75)
)

iqr = q3 - q1

lower = q1 - 1.5 * iqr
upper = q3 + 1.5 * iqr

within_outliers = clean[
    (clean['body_mass_g'] < lower) |
    (clean['body_mass_g'] > upper)
]

print('Outliers within species:', len(within_outliers))
```

```python
# 3. Display the unusual penguins
within_outliers.sort_values(
    ['species', 'body_mass_g']
)
```

### 🤖 Explain It

In your own words: explain to a classmate why "outlier" and "error" aren't the same thing, using an example from the Titanic fares.

- An outlier and an error are not the same thing. An outlier is simply a value that is very different from most other values. For example, a Titanic passenger paying a very expensive fare might be an outlier, but the fare could be completely correct. We should investigate unusual values instead of automatically deleting them.


---

## 👥 Section 4: Comparing Groups

7. When should you compare groups with counts, and when with rates?  
   `Answer:` __Use counts when you want to know the total number of observations in each group. Use rates when you want to compare proportions or percentages, especially when groups have different sizes__

8. What's the difference between `normalize='index'` and `normalize='columns'` in `pd.crosstab()`? Write a question each one answers.  
   `Answer:`

   - normalize='index': Each row adds up to 100%. It answers questions such as: What percentage of passengers in each class were male or female?
   - normalize='columns': Each column adds up to 100%. It answers questions such as: What percentage of male and female passengers belonged to each class?

---

### ✏️ Task: Survival Table

```python
# Using titanic.csv:
# 1. Bin Age into child (0-12), teen (13-18), adult (19-60), senior (61+) with pd.cut().
# 2. Build a pivot_table of survival rate with age group as rows and Sex as columns.
# 3. Add the group sizes (a second pivot_table with aggfunc='size').
# Which cell has the highest rate? Is its group big enough to trust?

# 1. Create age groups
titanic['AgeGroup'] = pd.cut(
    titanic['Age'],
    bins=[0, 12, 18, 60, np.inf],
    labels=['child', 'teen', 'adult', 'senior'],
    include_lowest=True
)

# 2. Calculate survival rate by age group and sex
survival_rate = titanic.pivot_table(
    values='Survived',
    index='AgeGroup',
    columns='Sex',
    aggfunc='mean',
    observed=True
)

print('Survival rates:')
survival_rate.round(3)
```

```python
# 3. Calculate the number of passengers in each group
group_sizes = titanic.pivot_table(
    values='Survived',
    index='AgeGroup',
    columns='Sex',
    aggfunc='size',
    observed=True
)

print('Group sizes:')
group_sizes
```

```python
print('Survival rates:')
print(survival_rate.round(3))

print('\nGroup sizes:')
print(group_sizes)

# Find the highest survival rate
highest = survival_rate.stack().idxmax()

print('\nHighest survival rate:')
print('Age group:', highest[0])
print('Sex:', highest[1])
print('Rate:', round(survival_rate.loc[highest[0], highest[1]], 3))

print('Group size:', group_sizes.loc[highest[0], highest[1]])
```

### 🤖 Explain It

In your own words: what's a confounder? Describe one in the Titanic data, and how you'd check for it with a table.

- A confounder is a third variable that influences or is related to both variables being studied, making their relationship potentially misleading. In the Titanic dataset, passenger class could be a confounder when studying the relationship between port of embarkation and survival. Some ports had different distributions of passenger classes, and passenger class was related to survival. We can investigate this by creating a table that compares survival rates by both port and passenger class

---

## 🔗 Section 5: Relationships and Correlation

9. When would you use Spearman instead of Pearson?  
   `Answer:` _I would use Spearman when working with ordinal data, when the relationship is curved but consistently increasing or decreasing, or when outliers make Pearson correlation less reliable. Spearman uses ranks instead of the original values__

10. A news story says "cities with more ice-cream sales have more drownings." Give a likely confounder.  
    `Answer:` A likely confounder is temperature or season. During warmer months, people buy more ice cream and also spend more time swimming, which can increase drownings. Ice-cream sales do not directly cause drownings__

---

### ✏️ Task: Correlation Report

```python
# Using pokemon.csv:
# 1. Compute the Pearson and Spearman correlation matrices of the six stats.
# 2. List the three strongest pairs (use the upper triangle).
# 3. Recompute r(Attack, Defense) for Legendary and non-Legendary separately.

# Six Pokémon stats
stats = ['HP', 'Attack', 'Defense', 'Sp. Atk', 'Sp. Def', 'Speed']

# 1. Pearson correlation matrix
pearson_corr = pokemon[stats].corr(method='pearson')

print('Pearson correlation:')
print(pearson_corr.round(3))
```

```python
# Spearman correlation matrix
spearman_corr = pokemon[stats].corr(method='spearman')

print('Spearman correlation:')
print(spearman_corr.round(3))
```
```python
# 2. Keep only the upper triangle
upper = pearson_corr.where(
    np.triu(
        np.ones(pearson_corr.shape, dtype=bool),
        k=1
    )
)

# Convert to one list of pairs and sort by strength
strongest_pairs = (
    upper.stack()
    .rename('r')
    .sort_values(key=abs, ascending=False)
)

print('Three strongest Pearson pairs:')
print(strongest_pairs.head(3).round(3))
```
```python
spearman_upper = spearman_corr.where(
    np.triu(
        np.ones(spearman_corr.shape, dtype=bool),
        k=1
    )
)

strongest_spearman = (
    spearman_upper.stack()
    .rename('rho')
    .sort_values(key=abs, ascending=False)
)

print('Three strongest Spearman pairs:')
print(strongest_spearman.head(3).round(3))
```

```python
# 3. Legendary Pokémon
legendary = pokemon[pokemon['Legendary'] == True]

r_legendary = legendary['Attack'].corr(
    legendary['Defense']
)

# Non-Legendary Pokémon
non_legendary = pokemon[pokemon['Legendary'] == False]

r_non_legendary = non_legendary['Attack'].corr(
    non_legendary['Defense']
)

print('Legendary Attack/Defense correlation:', round(r_legendary, 3))
print('Non-Legendary Attack/Defense correlation:', round(r_non_legendary, 3))
```




### 🤖 Explain It

In your own words: Anscombe's four datasets all have r = 0.82. What does that teach you about reporting a correlation without a plot? What's the danger of a single summary number?

- Anscombe's quartet contains four datasets with nearly identical summary statistics and the same correlation of approximately 0.82, but their actual distributions look very different. This teaches us that reporting only a correlation can hide important patterns, such as curves or outliers. A single summary number does not show the entire relationship, so we should also visualize the data before drawing conclusions

---

## 🧾 Submit Checklist

- [x] I built a data dictionary that separates dtype from measurement scale.
- [x] I used an ordered category and computed a proportion from a boolean column.
- [x] I reported center and spread that fit a column's shape, and explained `ddof`.
- [x] I flagged outliers with the IQR rule, inspected them, and decided what to do with them.
- [x] I compared groups with sizes and rates, using a crosstab and a pivot table.
- [x] I computed Pearson and Spearman correlations and explained a possible confounder.
- [x] I completed the "Explain It" prompts in my own words.
