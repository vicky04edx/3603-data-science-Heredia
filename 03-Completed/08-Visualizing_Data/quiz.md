# 📝 Quiz: 08 - Visualizing Data

36 questions covering object-oriented Matplotlib, plotting with pandas, Seaborn distributions, Seaborn relationships and categories, and choosing and designing charts. Mix of multiple choice, true/false, code-tracing, and short answer.

Try every question on your own first. The Answer Key is at the bottom, no peeking. If you're using an AI tutor, paste in *your answer* and ask it to check your reasoning rather than asking it to solve the question first.

---

## Section A — Matplotlib, Object-Oriented

**1.** (Short Answer) In `fig, ax = plt.subplots()`, what's the difference between `fig` and `ax`?

Answer: fig is the entire figure or canvas. ax is the individual axes where the chart is drawn, including its plotting area, labels, and ticks.

**2.** (Short Answer) Why does the object-oriented style scale better than `plt.title()`/`plt.xlabel()` calls when a figure has several charts?

Answer: The object-oriented style lets you control each chart independently. With multiple charts, you can specify exactly which axes receives a title, label, or other setting instead of accidentally changing the wrong chart.

**3.** (Code Tracing) How many axes does this create, and how do you reach the bottom-right one?
```python
fig, axes = plt.subplots(2, 3)
```

Answer: It creates 6 axes, arranged in 2 rows and 3 columns. You reach the bottom-right one with axes[1, 2]

**4.** (Short Answer) What does `sharey=True` do, and why does it matter when comparing panels?

Answer: sharey=True makes all panels use the same y-axis scale. This matters because it allows us to compare values fairly without different axis ranges making one panel look larger or smaller than another.

**5.** (Short Answer) When is a log-scale axis a good idea? What value can't it show?

Answer: A log scale is useful when values cover a very large range or when we want to compare multiplicative changes. It cannot show zero or negative values.

**6.** (Multiple Choice) Which formatter displays `0.25` on an axis as `25%`?
A) `StrMethodFormatter('{x:.0%}')`  B) `PercentFormatter(xmax=1)`  C) `PercentFormatter(xmax=100)`  D) `ax.set(percent=True)`

Answer: B

**7.** (Short Answer) Why should you call `fig.savefig()` before `plt.show()`, and why `plt.close(fig)` inside a loop?

Answer: Calling fig.savefig() before plt.show() helps ensure the figure is saved before it is displayed or potentially cleared by the plotting environment. Calling plt.close(fig) inside a loop releases the figure from memory and prevents too many open figures from accumulating.

**8.** (Short Answer) In `ax.annotate('Peak', xy=(5, 10), xytext=(7, 12), arrowprops=...)`, what do `xy` and `xytext` each mean?

Answer: 
- xy is the point on the chart that the arrow points to.
- xytext is the position where the annotation text (Peak) appears.
---

## Section B — Plotting with Pandas

**9.** (Short Answer) When pandas plots a DataFrame, what becomes the x-axis, and what becomes each line or set of bars?

Answer: The DataFrame's index becomes the x-axis, and each numeric column becomes a separate line or set of bars.

**10.** (Code Tracing) What kind of chart does this draw, and what are the bar labels?
```python
titanic['Embarked'].value_counts().plot.barh()
```

Answer: It draws a horizontal bar chart. The bar labels are the unique categories in the Embarked column, such as S, C, and Q. The bar lengths represent how many times each category appears.

**11.** (Short Answer) You plot a `pivot_table` with classes as the index and sexes as the columns. How do you switch the chart to group by sex instead?

Answer:  Transpose the pivot table using .T before plotting: pivot_table.T.plot.bar(). This switches the index and columns, so the chart groups by sex instead of class.

**12.** (Short Answer) What does a 100% stacked bar chart show, and which `crosstab` argument produces the right table for it?

Answer: A 100% stacked bar chart shows the percentage breakdown of categories within each group. The normalize='index' argument in pd.crosstab() produces the correct table: pd.crosstab(df['Group'], df['Category'], normalize='index')

**13.** (Short Answer) What's the difference between wide and long data? Which does pandas `.plot` expect, and which does Seaborn expect?

Answer: 
- Wide data: Each variable has its own column, with observations organized across rows.

- Long data: Observations are organized into rows, with columns identifying the variable and its value.

**14.** (Short Answer) Should you sort the bars of an ordinal category (like ticket class) 

Answer: No. Ordinal categories have a meaningful natural order, such as ticket classes 1st, 2nd, and 3rd. We should preserve that order instead of sorting by value because it makes the categories easier to interpret.
---

## Section C — Seaborn: Distributions

**15.** (Short Answer) What's the difference between an axes-level and a figure-level Seaborn function? Give one example of each.

Answer:

- Axes-level functions draw a chart on one existing Matplotlib axes. Example: sns.histplot().
- Figure-level functions manage an entire figure and can create multiple panels. Example: sns.displot().

**16.** (Short Answer) What does `hue='species'` do in `sns.histplot()`?

Answer: hue='species' separates the data by species and gives each species a different color, allowing us to compare their distributions.

**17.** (Multiple Choice) Two groups have 500 and 50 rows. To compare the **shapes** of their distributions, use:
A) `stat='count'`  B) `multiple='stack'`  C) `stat='percent', common_norm=False`  D) `binwidth=1`

Answer: C

**18.** (Short Answer) Explain each part of a box plot: the box, the line inside it, the whiskers, and the dots.

- Box: Represents the middle 50% of the data, from the first quartile (Q1) to the third quartile (Q3).
- Line inside: Represents the median (Q2).
- Whiskers: Extend to the most extreme values within 1.5 times the interquartile range (IQR) from the box.
- Dots: Represent individual outliers beyond the whiskers.

**19.** (Short Answer) Name one thing a violin plot shows that a box plot hides, and one thing a strip plot shows that a violin hides.

Answer: A violin plot shows the shape and density of the distribution, including where values are concentrated. A strip plot shows the individual observations as dots, which a violin plot hides.

**20.** (Short Answer) How do you read a group's median from an ECDF plot?

Answer: Find 50% (0.5) on the y-axis and look across to where it meets the ECDF curve. The corresponding x-axis value is the group's median.

**21.** (True/False) A KDE curve always stays within the actual range of the data.

Answer: False

**22.** (Short Answer) Why does `showfliers=False` make sense when you draw a strip plot on top of a box plot?

Answer: showfliers=False prevents the box plot from displaying outlier dots because the strip plot already shows individual observations. This avoids displaying the same points twice and makes the chart less cluttered.

---

## Section D — Seaborn: Relationships and Categories

**23.** (Short Answer) Name two ways to reduce overplotting in a scatter plot.

Answer:

- Use transparency with alpha, so overlapping points become easier to notice.
- Use smaller markers with s to reduce how much space each point occupies.

**24.** (Short Answer) In the penguins data, one `regplot` line through all the points slopes down, but one line per species slopes up. What's this called, and what's the lesson for fitting trend lines?

Answer: This is called Simpson's paradox. It happens when a trend in combined data is different from the trends within individual groups. The lesson is that we should consider important categories before fitting or interpreting trend lines.

**25.** (Short Answer) Why did the four Anscombe datasets get identical lines from `lmplot`?

Answer: The four Anscombe datasets have nearly identical statistical summaries, including similar regression lines, even though their actual data distributions are very different. This demonstrates why visualizing data is important instead of relying only on summary statistics.

**26.** (Short Answer) What's the difference between `countplot` and `barplot`?

Answer: 
- countplot() counts how many observations belong to each category.
- barplot() calculates and displays a statistical estimate (the mean by default) for a numeric variable within each category.

**27.** (Multiple Choice) `sns.barplot(..., errorbar='sd')` draws error bars showing:
A) the 95% confidence interval  B) the standard error  C) one standard deviation of the individual values  D) the IQR

Answer: C

**28.** (Short Answer) What does Seaborn's default error bar on a `barplot` represent?

Answer: Seaborn's default error bar on a barplot represents a 95% confidence interval around the estimated mean. It shows the uncertainty in the estimated mean, not the spread of individual observations.

**29.** (Short Answer) What does a `pointplot` with crossing or diverging lines tell you?

Answer: Crossing or diverging lines in a point plot can indicate an interaction between variables. This means the relationship or difference between groups changes depending on the category on the x-axis.

**30.** (Short Answer) Should a correlation heatmap use a diverging or a sequential color map? What about a heatmap of survival rates? Why?

Answer: Correlation heatmap: Use a diverging color map because correlations range from negative to positive, with zero as the midpoint. Survival-rate heatmap: Use a sequential color map because survival rates go from low to high, representing one direction of magnitude.

---

## Section E — Choosing and Telling

**31.** (Short Answer) Why must bar charts start at zero, when line charts don't have to?

Answer: Bar charts represent quantities through the length of their bars, so they should start at zero to avoid exaggerating differences. Line charts show changes and trends, so their axes can use a narrower range when appropriate, as long as the scale is clear

**32.** (Short Answer) Name the chart you'd choose for each: (a) the distribution of one numeric column, (b) the mix of categories within each group, (c) the relationship between two numeric columns.

Answer: 
- (a) Distribution of one numeric column: Histogram.
- (b) Mix of categories within each group: 100% stacked bar chart.
- (c) Relationship between two numeric columns: Scatter plot.
**33.** (Short Answer) What's a palette dictionary, and why use one across a whole report?

Answer: A palette dictionary assigns a specific color to each category. Using the same dictionary throughout a report keeps categories consistent, making charts easier to understand and compare

**34.** (Short Answer) Rewrite this descriptive title as a finding title: "Mean Body Mass by Species."

Answer: "Gentoo Penguins Have a Higher Mean Body Mass Than Adelie and Chinstrap Penguins." A finding title communicates the main takeaway instead of simply describing what the chart displays.

**35.** (True/False) Every bar in a bar chart should have its own distinct color.

Answwr: False

**36.** (Short Answer) Give two reasons to look at a chart **and** the matching numbers, rather than just one of them.

Answer:
- Charts reveal patterns: They help us quickly notice trends, differences, outliers, and relationships that may be difficult to see in a table.
- Numbers provide precision: They give us exact values and statistics that help confirm what we see in the chart and avoid making incorrect conclusions.