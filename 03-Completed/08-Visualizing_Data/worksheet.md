# 📝 Worksheet: 08 - Visualizing Data

Use this worksheet to reinforce object-oriented Matplotlib, pandas plotting, Seaborn, and chart design. Each section ends with an **🤖 Explain It** prompt. Write your explanation in your own words, then (optionally) paste it into an AI tutor and ask it to point out anything you got wrong or left out.

---

## 🖼️ Section 1: Matplotlib, Object-Oriented

1. What's the difference between `plt.title('X')` and `ax.set_title('X')`? When does the difference matter?  
   `Answer:` __plt.title('X') sets the title of the currently active chart, while ax.set_title('X') sets the title of a specific axes. The difference matters when a figure has multiple charts because using ax.set_title() makes sure we change the correct chart.__

2. You're comparing three histograms in a 1 × 3 figure. Name two settings that make the panels fair to compare.  
   `Answer:` 

   - Use the same x-axis limits (sharex=True) so the values are compared on the same scale.
   - Use the same y-axis limits (sharey=True) so the heights of the histograms can be compared fairly.

---

### ✏️ Task: A Reusable Chart Function

```python
# Write plot_distribution(ax, series, title, unit) that draws a histogram on ax,
# marks the mean and median with vertical lines, and labels everything.
# Use it to build a 2 x 2 figure of Titanic Age, Fare (log scale!), SibSp, and Parch.
# Save the figure with fig.savefig().

import pandas as pd
import matplotlib.pyplot as plt

# Load Titanic dataset
titanic = pd.read_csv('titanic.csv')

# Create a reusable function
def plot_distribution(ax, series, title, unit):

    # Remove missing values
    series = series.dropna()

    # Draw histogram
    ax.hist(series, bins=20, edgecolor='black', alpha=0.7)

    # Calculate mean and median
    mean_value = series.mean()
    median_value = series.median()

    # Draw mean and median lines
    ax.axvline(mean_value, color='red', linestyle='--',
               label=f'Mean: {mean_value:.2f}')

    ax.axvline(median_value, color='green', linestyle='-',
               label=f'Median: {median_value:.2f}')

    # Add labels and title
    ax.set_title(title)
    ax.set_xlabel(unit)
    ax.set_ylabel('Frequency')

    # Add legend
    ax.legend()

# Create a 2 x 2 figure
fig, axes = plt.subplots(2, 2, figsize=(12, 8))

# Plot each distribution
plot_distribution(axes[0, 0], titanic['Age'], 'Titanic Age', 'Age (years)')

plot_distribution(axes[0, 1], titanic['Fare'], 'Titanic Fare', 'Fare')

plot_distribution(axes[1, 0], titanic['SibSp'], 'Number of Siblings/Spouses', 'SibSp')

plot_distribution(axes[1, 1], titanic['Parch'], 'Number of Parents/Children', 'Parch')

# Apply log scale to Fare chart
axes[0, 1].set_xscale('log')

# Add overall title
fig.suptitle('Distributions of Titanic Passenger Variables', fontsize=16)

# Adjust spacing
fig.tight_layout()

# Save figure
fig.savefig('titanic_distributions.png', dpi=300, bbox_inches='tight')

plt.show()

# Close figure
plt.close(fig)
```

### 🤖 Explain It

In your own words: why is a function that takes an `ax` argument more useful than a function that calls `plt.` directly?

- A function that takes an ax argument is more useful because it lets us choose exactly which chart to draw on. This makes the function reusable when creating figures with multiple panels. If we use plt. directly, we depend on whichever chart is currently active, which can become confusing when working with multiple charts.

---

## 🐼 Section 2: Plotting with Pandas

3. When you call `.plot.bar()` on a DataFrame, what does the index become, and what do the columns become?  
   `Answer:` __The DataFrame index becomes the x-axis categories, and each column becomes a separate group of bars__

4. Give one example of wide data and the same data in long form.  
   `Answer:`

   Wide format:
   |Generation |	Legendary | Not Legendary|
   |1	 | 6 | 160|
   |2 |	5 |	95|

   Long format:
   |Generation |	Legendary | Count|
   |1	 | yes| 6|
   |1 |	no|	160|
   |2	 | yes| 5|
   |2 |	no |	95|
---

### ✏️ Task: Summary Table to Chart

```python
# Using pokemon.csv:
# 1. Build a crosstab of Generation vs. Legendary, normalized by row.
# 2. Plot it as a 100% stacked bar chart with a percent-formatted axis.
# 3. Melt the (un-normalized) crosstab into long form with columns Generation, Legendary, count.

import pandas as pd
import matplotlib.pyplot as plt
from matplotlib.ticker import PercentFormatter

# Load Pokemon dataset
pokemon = pd.read_csv('pokemon.csv')

# 1. Build a crosstab of Generation vs. Legendary
pokemon_table = pd.crosstab(
    pokemon['Generation'],
    pokemon['Legendary']
)

print("Original Crosstab:")
display(pokemon_table)

# 2. Normalize by row to get percentages
pokemon_percent = pd.crosstab(
    pokemon['Generation'],
    pokemon['Legendary'],
    normalize='index'
)

print("Normalized Crosstab:")
display(pokemon_percent)

# Plot as a 100% stacked bar chart
ax = pokemon_percent.plot(
    kind='bar',
    stacked=True,
    figsize=(9, 5),
    color=['#4C72B0', '#DD8452'],
    edgecolor='black'
)

# Format y-axis as percentages
ax.yaxis.set_major_formatter(PercentFormatter(1))

# Add labels and title
ax.set_title('Legendary vs. Non-Legendary Pokémon by Generation')
ax.set_xlabel('Generation')
ax.set_ylabel('Percentage')
ax.legend(title='Legendary')

plt.tight_layout()
plt.savefig('pokemon_generation_stacked.png', dpi=300)
plt.show()

# 3. Melt the original (un-normalized) crosstab into long form
pokemon_long = pokemon_table.reset_index().melt(
    id_vars='Generation',
    var_name='Legendary',
    value_name='count'
)

print("Long Format Data:")
display(pokemon_long)
```

### 🤖 Explain It

In your own words: when would you plot with pandas `.plot`, and when would you switch to Seaborn?

- I would use pandas .plot() when I already have a summary table and want to create a simple chart quickly. I would switch to Seaborn when I want to explore relationships between variables, compare distributions across groups, or create more advanced statistical visualizations using the original data.

---

## 🌊 Section 3: Seaborn Distributions

5. Why can a histogram with `hue` and raw counts be misleading when the groups are very different sizes? What's the fix?  
   `Answer:` _Raw counts can make a larger group look more important simply because it has more observations. It becomes difficult to compare the actual shapes of the distributions. The fix is to use stat='percent' and common_norm=False, which calculates percentages separately for each group.__

6. Explain how a box plot's whiskers and dots connect to the 1.5 × IQR rule from Module 07.  
   `Answer:` __The IQR is the distance between the third quartile (Q3) and the first quartile (Q1). The whiskers extend to the most extreme data points that are still within 1.5 × IQR below Q1 or above Q3. Values outside those limits are displayed as individual dots called outliers.__

---

### ✏️ Task: Compare Distributions Four Ways

```python
# Using penguins.csv, for flipper_length_mm by species, make a 2 x 2 figure:
# a step histogram with hue, a KDE with hue, a box plot, and a violin plot.
# Use the same species order and the same colors in every panel.

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load penguins dataset
penguins = pd.read_csv('penguins.csv')

# Remove missing values needed for this chart
penguins = penguins.dropna(subset=['flipper_length_mm', 'species'])

# Set species order
species_order = ['Adelie', 'Chinstrap', 'Gentoo']

# Use the same colors for every chart
species_colors = {
    'Adelie': '#4C72B0',
    'Chinstrap': '#DD8452',
    'Gentoo': '#55A868'
}

# Create a 2 x 2 figure
fig, axes = plt.subplots(2, 2, figsize=(14, 10))

# 1. Step histogram with hue
sns.histplot(
    data=penguins,
    x='flipper_length_mm',
    hue='species',
    hue_order=species_order,
    palette=species_colors,
    element='step',
    fill=False,
    common_norm=False,
    ax=axes[0, 0]
)

axes[0, 0].set_title('Step Histogram of Flipper Length')
axes[0, 0].set_xlabel('Flipper Length (mm)')
axes[0, 0].set_ylabel('Count')

# 2. KDE with hue
sns.kdeplot(
    data=penguins,
    x='flipper_length_mm',
    hue='species',
    hue_order=species_order,
    palette=species_colors,
    common_norm=False,
    ax=axes[0, 1]
)

axes[0, 1].set_title('KDE of Flipper Length')
axes[0, 1].set_xlabel('Flipper Length (mm)')
axes[0, 1].set_ylabel('Density')

# 3. Box plot
sns.boxplot(
    data=penguins,
    x='species',
    y='flipper_length_mm',
    order=species_order,
    hue='species',
    hue_order=species_order,
    palette=species_colors,
    legend=False,
    ax=axes[1, 0]
)

axes[1, 0].set_title('Box Plot of Flipper Length')
axes[1, 0].set_xlabel('Species')
axes[1, 0].set_ylabel('Flipper Length (mm)')

# 4. Violin plot
sns.violinplot(
    data=penguins,
    x='species',
    y='flipper_length_mm',
    order=species_order,
    hue='species',
    hue_order=species_order,
    palette=species_colors,
    legend=False,
    ax=axes[1, 1]
)

axes[1, 1].set_title('Violin Plot of Flipper Length')
axes[1, 1].set_xlabel('Species')
axes[1, 1].set_ylabel('Flipper Length (mm)')

# Make the figure look organized
fig.suptitle('Comparing Penguin Flipper Length Distributions', fontsize=16)
fig.tight_layout()

# Save figure
fig.savefig('penguin_flipper_distributions.png', dpi=300, bbox_inches='tight')

plt.show()
plt.close(fig)
```

### 🤖 Explain It

In your own words: the overall penguin body-mass histogram looked like one lumpy pile, and its skew was only 0.47. What did coloring by species reveal, and why couldn't the skew statistic reveal it?

- Coloring by species revealed that the penguins actually form different groups with different body-mass distributions. Gentoo penguins tend to be much heavier than Adelie and Chinstrap penguins. The overall histogram looked like one large, slightly skewed distribution because all species were mixed together. The skew statistic only summarizes the overall shape with one number, so it cannot show separate groups or explain what is causing the shape.

---

## 🔍 Section 4: Seaborn Relationships and Categories

7. What do the error bars on a default `sns.barplot()` mean? How would `errorbar='sd'` change them?  
   `Answer:` __By default, Seaborn's bar plot displays a 95% confidence interval around the estimated mean. If we use errorbar='sd', the error bars instead show one standard deviation of the individual observations. The confidence interval represents uncertainty in the mean, while standard deviation describes the spread of the data._

8. Why should you color a scatter plot by group before adding a trend line?  
   `Answer:` __Coloring the scatter plot by group helps us see whether different groups have different patterns or relationships. If we only look at one trend line through all the points, we might miss those differences or even reach the wrong conclusion. This can help reveal Simpson's paradox._

---

### ✏️ Task: Reveal the Paradox

```python
# Using penguins.csv:
# 1. Draw one regplot of bill_length_mm vs. bill_depth_mm for all penguins.
# 2. Draw one regplot per species on a second panel, with r in each label.
# 3. Give each panel a title that states what it shows.
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load penguins dataset
penguins = pd.read_csv('penguins.csv')

# Keep only rows with the needed information
data = penguins.dropna(
    subset=['bill_length_mm', 'bill_depth_mm', 'species']
)

# Set species colors
species_colors = {
    'Adelie': '#4C72B0',
    'Chinstrap': '#DD8452',
    'Gentoo': '#55A868'
}

# Create two panels
fig, axes = plt.subplots(1, 2, figsize=(14, 6), sharex=True, sharey=True)

# 1. Overall regression line
sns.regplot(
    data=data,
    x='bill_length_mm',
    y='bill_depth_mm',
    scatter_kws={'alpha': 0.5},
    line_kws={'color': 'black'},
    ax=axes[0]
)

axes[0].set_title('All Penguins: Overall Relationship')
axes[0].set_xlabel('Bill Length (mm)')
axes[0].set_ylabel('Bill Depth (mm)')

# 2. Regression line for each species
for species in ['Adelie', 'Chinstrap', 'Gentoo']:

    # Select one species
    group = data[data['species'] == species]

    # Calculate correlation
    r = group['bill_length_mm'].corr(group['bill_depth_mm'])

    # Draw regression line and scatter points
    sns.regplot(
        data=group,
        x='bill_length_mm',
        y='bill_depth_mm',
        scatter_kws={
            'alpha': 0.4,
            'color': species_colors[species]
        },
        line_kws={'color': species_colors[species]},
        ax=axes[1],
        label=f'{species} (r = {r:.2f})'
    )

axes[1].set_title('Each Species: Individual Relationships')
axes[1].set_xlabel('Bill Length (mm)')
axes[1].set_ylabel('Bill Depth (mm)')
axes[1].legend(title='Species')

# Overall title
fig.suptitle('Penguin Bill Length vs. Bill Depth', fontsize=16)

fig.tight_layout()

# Save figure
fig.savefig('penguin_bill_paradox.png', dpi=300, bbox_inches='tight')

plt.show()
plt.close(fig)
```

### 🤖 Explain It

In your own words: explain Simpson's paradox to someone who has never heard of it, using your chart from the task above.

- Simpson's paradox happens when a trend in the combined data is different from the trends inside individual groups. In our chart, the overall regression line can show a negative relationship between bill length and bill depth, while the individual species lines show positive relationships. This happens because the species have different average bill sizes and depths. The lesson is that combining different groups can hide or even reverse the patterns inside those groups, so we should look at the data by category before making conclusions.

---

## 🗣️ Section 5: Choosing and Telling

9. Name three things a bar chart's y-axis can do to mislead a reader.  
   `Answer:`
   - tart above zero, making small differences between bars look much bigger.
   - Use a very large range, making important differences look insignificant.
   - Use inconsistent scales across charts, making comparisons unfair.

10. What's the difference between a descriptive title and a finding title? Write one of each for the same chart.  
    `Answer:` 
    - Descriptive title: Mean Body Mass by Penguin Species.
    - Finding title: Gentoo Penguins Have a Higher Mean Body Mass Than Other Species.

---

### ✏️ Task: Makeover

```python
# Start with: titanic.groupby('Embarked')['Fare'].mean().plot.bar()
# Redesign it with the notebook 05 checklist: the right statistic for skewed data,
# readable port names, sorted bars, a highlight color, bar labels, and a finding title.
# Save the before and after versions.

import pandas as pd
import matplotlib.pyplot as plt

# Load Titanic dataset
titanic = pd.read_csv('titanic.csv')

# --------------------------------
# BEFORE: Original chart
# --------------------------------

fig, ax = plt.subplots(figsize=(8, 5))

titanic.groupby('Embarked')['Fare'].mean().plot.bar(
    ax=ax,
    color='steelblue'
)

ax.set_title('Mean Fare by Embarked')
ax.set_xlabel('Embarked')
ax.set_ylabel('Mean Fare')

plt.tight_layout()

# Save before version
fig.savefig('titanic_fare_before.png', dpi=300, bbox_inches='tight')

plt.show()
plt.close(fig)


# --------------------------------
# AFTER: Improved chart
# --------------------------------

# Calculate median fare by port
# Median is better because Fare is right-skewed
fare_by_port = titanic.groupby('Embarked')['Fare'].median()

# Replace port codes with readable names
fare_by_port.index = fare_by_port.index.map({
    'C': 'Cherbourg',
    'Q': 'Queenstown',
    'S': 'Southampton'
})

# Sort bars from lowest to highest
fare_by_port = fare_by_port.sort_values()

# Highlight the port with the highest median fare
colors = [
    '#55A868' if value == fare_by_port.max() else '#4C72B0'
    for value in fare_by_port
]

# Create improved chart
fig, ax = plt.subplots(figsize=(9, 6))

bars = ax.bar(
    fare_by_port.index,
    fare_by_port.values,
    color=colors,
    edgecolor='black'
)

# Add labels above bars
ax.bar_label(
    bars,
    fmt='%.2f',
    padding=4
)

# Add finding title
ax.set_title(
    'Passengers Embarking at Cherbourg Paid the Highest Median Fare',
    fontsize=12
)

ax.set_xlabel('Port of Embarkation')
ax.set_ylabel('Median Fare')

# Start y-axis at zero
ax.set_ylim(0, fare_by_port.max() * 1.15)

# Remove unnecessary top and right borders
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.tight_layout()

# Save after version
fig.savefig('titanic_fare_after.png', dpi=300, bbox_inches='tight')

plt.show()
plt.close(fig)
```

### 🤖 Explain It

In your own words: what's the difference between a chart made for exploration and a chart made for communication? Why do they need different amounts of polish?

- An exploration chart is made to help us understand the data, discover patterns, and investigate relationships. It does not need to look perfect because its main purpose is helping us learn about the data. A communication chart is made to explain our findings to other people. It needs more polish, including clear titles, readable labels, appropriate colors, and an organized layout. This makes the main message easier to understand and prevents confusion.

---

## 🧾 Submit Checklist

- [x] I built a multi-panel Matplotlib figure with `fig, ax`, shared axes, and `fig.savefig()`.
- [x] I used a log scale, a reference line, and a tick formatter where they helped.
- [x] I turned a pandas summary table into a chart, and reshaped data between wide and long.
- [x] I compared distributions across groups fairly (`common_norm=False`, shared axes, sample sizes).
- [x] I showed a relationship with a scatter plot colored by group, and explained my error bars.
- [x] I redesigned a default chart using the checklist, with a finding title.
- [x] I completed the "Explain It" prompts in my own words.
