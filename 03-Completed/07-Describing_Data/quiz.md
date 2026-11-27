# 📝 Quiz: 07 - Describing Data

36 questions covering types of data, center and spread, shape and outliers, comparing groups, and relationships and correlation. Mix of multiple choice, true/false, code-tracing, and short answer.

Try every question on your own first. The Answer Key is at the bottom, no peeking. If you're using an AI tutor, paste in *your answer* and ask it to check your reasoning rather than asking it to solve the question first.

---

## Section A — Types of Data

**1.** (Short Answer) What's the difference between a column's **dtype** and its **measurement scale**?

- dtype describes how Python stores the data, such as int64, float64, str, or category.
- Measurement scale describes what the data means: nominal, ordinal, interval, or ratio.

**2.** (Multiple Choice) Titanic's `Pclass` is stored as `1`, `2`, `3`. Its measurement scale is:
A) Nominal  B) Ordinal  C) Interval  D) Ratio

Answer: B

**3.** (Multiple Choice) Which scale has a true zero that means "none of it"?
A) Nominal  B) Ordinal  C) Interval  D) Ratio

Answer: D

**4.** (True/False) Temperature in °C is ratio data, because 20°C is twice 10°C.

Answer: False

**5.** (Short Answer) Is "number of siblings aboard" discrete or continuous? How can you tell?

Answer: It is discrete because siblings are counted in whole numbers. You can have 1, 2, or 3 siblings, but not 2.5 siblings.

**6.** (Code Tracing) What prints?
```python
s = pd.Series([True, False, True, True])
print(s.mean())
```
Answer: 0.75

**7.** (Short Answer) Why is `titanic['PassengerId'].mean()` meaningless, even though pandas computes it without complaint?

Answer: Because PassengerId is an identifier, not a measurement. The numbers are used to identify passengers, not represent a meaningful quantity. Calculating their average does not tell us anything useful about the passengers.

**8.** (Short Answer) What does `ordered=True` in `pd.CategoricalDtype([...], ordered=True)` let you do that a plain `str` column can't?

Answer: It tells pandas that the categories have a meaningful order. This allows us to compare categories using their order, such as Low < Medium < High. A plain string column does not automatically understand that order
---

## Section B — Center and Spread

**9.** (Short Answer) Which is more robust to outliers, the mean or the median? Why?

Answer: The median is more robust because it uses the middle position of the sorted values. Extreme values have much less influence on it than on the mean.

**10.** (Code Tracing) What prints?
```python
s = pd.Series([2, 4, 4, 6, 100])
print(s.median(), s.mode()[0])
```
Answer: 4.0 4

**11.** (Short Answer) A column's mean is much larger than its median. What does that suggest?

Answer: It suggests the data is likely right-skewed. Some unusually large values pull the mean upward

**12.** (Multiple Choice) `pd.Series(data).std()` and `np.std(data)` give slightly different answers because:
A) NumPy rounds  B) pandas skips NaN  C) they use different default `ddof`  D) one is variance

Answer: C

**13.** (Short Answer) What's the IQR, and how do you compute it in pandas?

Answer: The Interquartile Range measures the spread of the middle 50% of the data. iqr = s.quantile(0.75) - s.quantile(0.25)

**14.** (Short Answer) In `.describe()`, a column's `count` is 714 but the DataFrame has 891 rows. What does that tell you?
Answer:It means there are 714 non-missing values and: 891-714=177 
There are 177 missing values in that column.

**15.** (Short Answer) You fill 177 missing ages with the median age. What happens to the standard deviation, and why?

Answer: The standard deviation generally decreases because the missing values are replaced with the median, which is near the center of the data. This adds values near the center and reduces the overall spread.

**16.** (Short Answer) Which spread statistic pairs naturally with the median, and which with the mean?

Answer: The median and IQR are robust to outliers, while the mean and standard deviation are more affected by extreme values
---

## Section C — Shape and Outliers

**17.** (Multiple Choice) A column with `.skew()` of 4.8 is:
A) roughly symmetric  B) strongly left-skewed  C) strongly right-skewed  D) heavy-tailed but symmetric

Answer: C

**18.** (Short Answer) Write the formulas for the lower and upper fences of the 1.5 × IQR rule.

Answer: Lower=Q1-1.5(IQR), Upper=Q3+1.5(IQR) Values below the lower fence or above the upper fence are flagged as potential outliers.

**19.** (Short Answer) On Titanic fares, the IQR rule flags 116 values but the |z| > 3 rule flags only 20. Why are z-scores less sensitive on a skewed column?

Answer: Z-scores use the mean and standard deviation. In a right-skewed distribution, extreme high values pull the mean upward and increase the standard deviation. This makes some unusually high values have smaller z-scores, so fewer are flagged.

**20.** (True/False) Any value flagged as an outlier should be removed before analysis.

Answer: False

**21.** (Short Answer) Name two things you can do with a real-but-extreme value instead of deleting it.

Answer: 

- Keep it but use robust statistics, such as the median and IQR.
- Transform the data, such as using a logarithm, to reduce the influence of extreme values.

**22.** (Short Answer) Why can a value be an outlier within its group but not in the whole dataset (or the other way around)?

Answer: Because different groups can have different distributions and typical values. A value might be normal when compared with everyone but unusual when compared only with members of its own group. The opposite can also happen: a value may look unusual overall because groups have different typical values, but be normal within its own group.
---

## Section D — Comparing Groups

**23.** (Short Answer) More 3rd-class passengers survived than 2nd-class passengers, but 3rd class had the lower survival rate. How can both be true?

Answer: Because the number of passengers in each class was different. Third class had more passengers overall, so it could have more survivors in total even though a smaller percentage survived.

**24.** (Code Tracing) What does this compute?
```python
titanic.groupby('Sex')['Survived'].mean()
```

Answer: It calculates the survival rate for each sex.

**25.** (Multiple Choice) In `pd.crosstab(df['class'], df['Sex'], normalize='index')`, what adds up to 100%?
A) each row  B) each column  C) the whole table  D) nothing; it's counts

Answer: A

**26.** (Short Answer) What's the difference between what `pd.crosstab()` and `pivot_table()` compute?

Answer: 
- pd.crosstab() summarizes the frequency or count of combinations between categorical variables.

- pivot_table() summarizes numeric values using an aggregation function such as mean, median, or sum.

**27.** (Short Answer) Why should a group summary always include the group's size?

Answer: Because the number of observations helps us understand how reliable the summary is. A mean calculated from 100 observations is generally more stable than one calculated from only 2 observations.

**28.** (Short Answer) Cherbourg passengers survived at a higher rate than other ports. Explain how ticket class could be a **confounder** here.

Answer: Cherbourg may have had more first-class passengers, who generally had higher survival rates. Ticket class affects both the port comparison and survival rates. Therefore, the higher survival rate at Cherbourg might be partly explained by its passenger class distribution rather than the port itself.

**29.** (Short Answer) What does `observed=True` do in a `groupby` on a categorical column?

Answer: It tells pandas to include only categories that actually appear in the data instead of also including unused categories.

---

## Section E — Relationships and Correlation

**30.** (Short Answer) Why is covariance hard to interpret on its own, and how does correlation fix that?

Answer: Covariance depends on the units and scales of the variables, so its size is difficult to interpret. Correlation standardizes covariance to a value between -1 and +1, making it easier to understand and compare relationships.

**31.** (Multiple Choice) r = −0.85 means:
A) a weak negative linear relationship  B) a strong negative linear relationship  C) 85% of y is caused by x  D) no relationship

ANswer: B

**32.** (Short Answer) Give two situations where Spearman is a better choice than Pearson.

Answer: 
- When working with ordinal data, such as rankings or passenger classes.

- When the relationship is curved but consistently increasing or decreasing, or when extreme outliers make Pearson less reliable.
**33.** (True/False) If r = 0, the two columns have no relationship at all.

Ansdwer: False

**34.** (Short Answer) Name three explanations for a correlation besides "A causes B."

Answer: 

- Reverse causation: B causes A instead.
- Confounding: A third variable causes both A and B.
- Coincidence: The correlation happened by chance.

**35.** (Short Answer) What is Anscombe's quartet, and what lesson does it teach?

Answer: Anscombe's quartet is a collection of four datasets with nearly identical descriptive statistics, including means, standard deviations, and correlations, but very different distributions and patterns. It teaches us that summary statistics alone do not tell the whole story. We should visualize data because different datasets can have the same statistics but completely different relationships.

**36.** (Short Answer) In the penguins data, bill length and depth correlate at −0.24 overall but +0.39 to +0.65 within each species. What's this called, and what causes it here?

Answer: This is called Simpson's paradox. It happens because the three penguin species have different physical characteristics. Gentoo penguins generally have longer but shallower bills compared with the other species. When all species are combined, this creates a negative correlation, even though each individual species has a positive correlation.
