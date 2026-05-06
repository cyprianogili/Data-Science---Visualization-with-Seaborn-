## Table of Contents

1. [What is Seaborn?](#1-what-is-seaborn)
2. [Seaborn vs Matplotlib](#2-seaborn-vs-matplotlib)
3. [Understanding Your Data - df.info()](#3-understanding-your-data---dfinfo)
4. [Types of Plots](#4-types-of-plots)
5. [Correlation and Covariance](#5-correlation-and-covariance)
6. [Correlation Does NOT Mean Causation](#6-correlation-does-not-mean-causation)

---

## 1. What is Seaborn?

Seaborn is a Python data visualization library built on top of Matplotlib.
It makes it easy to create beautiful, informative statistical charts with minimal code.

**Think of it this way:**

- Matplotlib = low-level painting tool. You control every single brush stroke.
- Seaborn = high-level visualization library. You describe what you want, it handles the details.

**Why Seaborn was created:**

- A plot that takes 8-10 lines in Matplotlib takes only 1-2 lines in Seaborn
- Seaborn automatically applies beautiful themes, sets good color palettes, adds grids and integrates with DataFrames naturally

```python
import seaborn as sns
import matplotlib.pyplot as plt
```

---

## 2. Seaborn vs Matplotlib

| | Matplotlib | Seaborn |
|---|---|---|
| Difficulty | More complex | Simpler |
| Code needed | More code | Less code |
| Default looks | Basic and plain | Beautiful |
| Control | Very high | Medium |
| Best for | Custom charts | Statistical charts |
| Built on | Itself | Matplotlib |

**How they work together:**

- Seaborn draws the chart
- Matplotlib styles and customizes it

```python
# Seaborn draws the chart
sns.scatterplot(data=df, x='Age', y='Fare', hue='Sex', size='Pclass')

# Matplotlib adds finishing touches
plt.title('Age vs Fare by Sex and Class')
plt.xlabel('Passenger Age (years)')
plt.ylabel('Fare Paid ($)')
plt.show()
```

---

## 3. Understanding Your Data - df.info()

`df.info()` is always your FIRST step with any new dataset. It tells you:

- How many rows you have
- How many columns you have
- Which columns have missing data
- What type of data each column contains

```python
df.info()
```

**Example output from Titanic dataset:**

```
RangeIndex: 891 entries, 0 to 890
Data columns (total 12 columns):
 #   Column       Non-Null Count  Dtype
---  ------       --------------  -----
 0   PassengerId  891 non-null    int64
 1   Survived     891 non-null    int64
 2   Pclass       891 non-null    int64
 3   Name         891 non-null    object
 4   Sex          891 non-null    object
 5   Age          714 non-null    float64
 6   SibSp        891 non-null    int64
 7   Parch        891 non-null    int64
 8   Ticket       891 non-null    object
 9   Fare         891 non-null    float64
 10  Cabin        204 non-null    object
 11  Embarked     889 non-null    object
```

**Data types explained:**

| Dtype | Meaning | Example |
|---|---|---|
| int64 | Whole numbers | 1, 2, 3 |
| float64 | Decimal numbers | 23.5, 7.25 |
| object | Text or String | "male", "John Smith" |

**How to check missing values directly:**

```python
df.isnull().sum()
```

**How to check shape of data:**

```python
df.shape
```

---

## 4. Types of Plots

### Quick Reference - Which Chart to Use?

| Question | Chart |
|---|---|
| How is ONE number column distributed? | Histogram or KDE |
| Relationship between TWO number columns? | Scatter Plot |
| General trend between TWO number columns? | Regression Plot |
| Distribution across categories? | Box Plot or Violin Plot |
| How many in each category? | Count Plot |
| All relationships at once? | Pair Plot |
| Correlation between all columns? | Heatmap |

---

### Scatter Plot

Used when you have TWO number columns and want to see if there is any relationship between them.

**Rule: Scatter plots work with NUMBERS ONLY!**

```python
plt.figure(figsize=(8, 6))
sns.scatterplot(data=df, x='Age', y='Fare', hue='Sex', size='Pclass')
plt.title('Age vs Fare by Sex and Class')
plt.xlabel('Passenger Age (years)')
plt.ylabel('Fare Paid ($)')
plt.show()
```

| Part | Simple Meaning |
|---|---|
| x='Age' | Put Age on the bottom axis |
| y='Fare' | Put Fare on the left axis |
| hue='Sex' | Color dots by Sex automatically |
| size='Pclass' | Change dot size by Pclass |

---

### Histogram

Shows the distribution of a numerical variable by dividing data into bins and counting observations in each bin.

```python
plt.figure(figsize=(8, 5))
sns.histplot(data=df, x='Age', kde=True, bins=20)
plt.title('Distribution of Age')
plt.xlabel('Age')
plt.ylabel('Count')
plt.grid(axis='y', alpha=0.75)
plt.show()
```

| Part | Simple Meaning |
|---|---|
| x='Age' | Use the Age column |
| kde=True | Add smooth curve on top |
| bins=20 | Divide ages into 20 groups |

---

### KDE Plot

KDE (Kernel Density Estimate) is a smooth curve that shows where most of your data is concentrated. It is smoother than histograms and not affected by bin sizes.

```python
plt.figure(figsize=(8, 5))
sns.kdeplot(data=df, x='Fare', fill=True)
plt.title('Density Plot of Fare')
plt.xlabel('Fare')
plt.ylabel('Density')
plt.grid(axis='y', alpha=0.75)
plt.show()
```

| Part | Simple Meaning |
|---|---|
| x='Fare' | Use the Fare column |
| fill=True | Fill the area under the curve with color |

**Simple rule:**

| Curve | Meaning |
|---|---|
| High curve | Many values at that point |
| Low curve | Few values at that point |
| Peak of curve | Most common value in your data |

---

### Regression Plot

Used to visualize the linear relationship (trend) between two variables. Draws a straight line through the dots showing the general direction.

```python
plt.figure(figsize=(8, 6))
sns.regplot(data=df, x='Age', y='Fare', scatter_kws={'alpha':0.3})
plt.title('Linear Relationship between Age and Fare')
plt.xlabel('Age')
plt.ylabel('Fare')
plt.show()
```

| Line Direction | Meaning |
|---|---|
| Goes UP | Positive trend - both increase together |
| Goes DOWN | Negative trend - one increases other decreases |
| Stays FLAT | No relationship |

**Two functions for regression:**

| Function | When to use |
|---|---|
| sns.regplot | Simple, quick analysis |
| sns.lmplot | More powerful, can split by groups using hue |

---

### Box Plot

Excellent for showing the distribution of a numerical variable across different categories. Shows median, quartiles and outliers.

```python
plt.figure(figsize=(10, 6))
sns.boxplot(data=df, x='Pclass', y='Age', hue='Survived')
plt.title('Age Distribution by Pclass and Survival')
plt.xlabel('Passenger Class')
plt.ylabel('Age')
plt.show()
```

**What each part means:**

| Part | Meaning |
|---|---|
| Middle line | Median - the middle value |
| The box | Where the middle 50% of data lives |
| Whiskers | Full range of normal values |
| Dots outside | Outliers - unusual values |
| Tall box | Data is very spread out |
| Short box | Data is concentrated together |

---

### Violin Plot

Combines Box Plot and KDE in one chart. Shows median, spread, outliers AND the full shape of the data distribution.

```python
plt.figure(figsize=(10, 6))
sns.violinplot(data=df, x='Pclass', y='Age', hue='Survived')
plt.title('Age Distribution by Pclass and Survival (Violin Plot)')
plt.show()
```

**Reading a Violin Plot:**

| Width | Meaning |
|---|---|
| Wide part | Many passengers at that age |
| Narrow part | Few passengers at that age |
| Widest point | Most common age in that group |

**Box Plot vs Violin Plot:**

| Feature | Box Plot | Violin Plot |
|---|---|---|
| Shows median | Yes | Yes |
| Shows spread | Yes | Yes |
| Shows outliers | Yes | Yes |
| Shows shape of data | No | Yes |

---

### Count Plot

Shows the count of observations in each category using bars. It is essentially a histogram for categorical variables.

```python
sns.countplot(data=df, x='Sex', hue='Survived')
plt.title('Count of Passengers by Sex')
plt.show()
```

**Simple rule:**

| Data Type | Chart to Use |
|---|---|
| Number columns | Histogram |
| Category columns | Count Plot |

**When to use Count Plot:**

| Question | Use Count Plot? |
|---|---|
| How many males vs females? | Yes |
| How many in each class? | Yes |
| How many survived vs died? | Yes |
| What is the average fare? | No |

---

### Pair Plot

Creates a grid of scatter plots for each pair of numerical variables. Histograms or KDE plots appear on the diagonal.

```python
sns.pairplot(df, hue='Survived')
plt.suptitle('Pairwise Relationships of Age, Fare, Pclass, and Survival')
plt.show()
```

**What it shows:**

| Position | Chart Type | Shows |
|---|---|---|
| Diagonal boxes | KDE or Histogram | Distribution of each individual column |
| All other boxes | Scatter Plot | Relationship between 2 columns |

---

### Heatmap

Turns a correlation matrix into colors so patterns are easy to see at a glance.

```python
plt.figure(figsize=(10, 8))
sns.heatmap(correlation_matrix, annot=True, fmt='.2f', cmap='coolwarm')
plt.title('Correlation Heatmap')
plt.show()
```

| Part | Simple Meaning |
|---|---|
| annot=True | Show the actual numbers inside each box |
| fmt='.2f' | Show numbers with 2 decimal places |
| cmap='coolwarm' | Use red and blue color scheme |

**Reading the colors:**

| Color | Value | Meaning |
|---|---|---|
| Dark Red | Close to +1 | Strong positive relationship |
| Light Pink | Around +0.2 | Weak positive relationship |
| White | Around 0 | No relationship |
| Light Blue | Around -0.2 | Weak negative relationship |
| Dark Blue | Close to -1 | Strong negative relationship |
| Diagonal | Always 1.0 | Every column is perfect with itself |

---

## 5. Correlation and Covariance

### Covariance

Covariance tells you if 2 things move in the same direction or opposite directions.

```python
# Step 1 - Select only number columns
numerical_df = df.select_dtypes(include='number')

# Step 2 - Calculate covariance
cov_matrix = numerical_df.cov()

# Step 3 - Display it
print(cov_matrix)
```

| Covariance Value | Meaning |
|---|---|
| Positive number | Both move in same direction |
| Negative number | They move in opposite directions |
| Near zero | No relationship |

**Problem with Covariance:**

- It is scale dependent
- Changing units changes the value
- Hard to compare different relationships
- That is why we use Correlation instead!

---

### Correlation

Correlation fixes the covariance problem by putting everything on a scale from -1 to +1.

```python
# Calculate correlation matrix
correlation_matrix = numerical_df.corr()
correlation_matrix
```

**The Correlation Scale:**

| Value | Meaning | Example |
|---|---|---|
| +1 | Perfect positive | Study hours vs Score |
| +0.5 | Moderate positive | Exercise vs Fitness |
| 0 | No relationship | Shoe size vs Score |
| -0.5 | Moderate negative | Absent days vs Score |
| -1 | Perfect negative | Speed vs Travel time |

**Key correlations from Titanic dataset:**

| Relationship | Value | Meaning |
|---|---|---|
| Survived vs Pclass | -0.338 | Poorer class = less survival |
| Survived vs Fare | +0.257 | Higher fare = more survival |
| Pclass vs Fare | -0.550 | Higher class number = cheaper fare |
| Pclass vs Age | -0.369 | Higher class number = younger passengers |
| SibSp vs Parch | +0.415 | Families travelled together |

**Simple 3 line rule:**

| Direction | Meaning |
|---|---|
| Close to +1 | They go UP together |
| Close to -1 | One goes UP other goes DOWN |
| Close to 0 | No relationship at all |

---

## 6. Correlation Does NOT Mean Causation

Correlation simply tells you how strongly two variables move together. It does NOT tell you why they move together.

**Famous example:**

- Ice cream sales go UP when drowning cases go UP
- Does ice cream CAUSE drowning? NO!
- Hot weather causes BOTH!

**Always ask WHY before concluding X causes Y!**

There could be:

- A hidden third factor causing both
- A pure coincidence
- A non-causal relationship

---

## Dataset Used

**Titanic Dataset - 891 passengers, 12 columns**

| Column | Description |
|---|---|
| PassengerId | Unique ID for each passenger |
| Survived | 0 = No, 1 = Yes |
| Pclass | Ticket class - 1 = First, 2 = Second, 3 = Third |
| Name | Passenger name |
| Sex | Male or Female |
| Age | Passenger age |
| SibSp | Number of siblings or spouses on board |
| Parch | Number of parents or children on board |
| Ticket | Ticket number |
| Fare | Ticket price |
| Cabin | Cabin number |
| Embarked | Port of embarkation - C, Q, S |

---

## Libraries Used

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
```

---

*Made with curiosity and a lot of questions* 😊
