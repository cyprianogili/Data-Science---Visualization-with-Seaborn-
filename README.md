# Data-Science---Visualization-with-Seaborn-
A beginner-friendly guide to data visualization using Seaborn and Matplotlib. Based on the Titanic dataset.

Table of Contents

What is Seaborn?
Seaborn vs Matplotlib
Understanding Your Data - df.info()
Types of Plots

Scatter Plot
Histogram
KDE Plot
Regression Plot
Box Plot
Violin Plot
Count Plot
Pair Plot
Heatmap
Correlation and Covariance
Correlation Does NOT Mean Causation

1. What is Seaborn?
Seaborn is a Python data visualization library built on top of Matplotlib.
It makes it easy to create beautiful, informative statistical charts with minimal code.
Think of it this way:

Matplotlib = low-level painting tool. You control every single brush stroke.
Seaborn = high-level visualization library. You describe what you want, it handles the details.

Why Seaborn was created:

A plot that takes 8-10 lines in Matplotlib takes only 1-2 lines in Seaborn
Seaborn automatically applies beautiful themes, sets good color palettes, adds grids and integrates with DataFrames naturally



```python
import seaborn as sns
import matplotlib.pyplot as plt

```


2. Seaborn vs Matplotlib
MatplotlibSeabornDifficultyMore complexSimplerCode neededMore codeLess codeDefault looksBasic and plainBeautifulControlVery highMediumBest forCustom chartsStatistical chartsBuilt onItselfMatplotlib
How they work together:

Seaborn draws the chart
Matplotlib styles and customizes it



# Seaborn draws the chart
sns.scatterplot(data=df, x='Age', y='Fare', hue='Sex', size='Pclass')

```Matplotlib adds finishing touches
plt.title('Age vs Fare by Sex and Class')
plt.xlabel('Passenger Age (years)')
plt.ylabel('Fare Paid ($)')
plt.show()

```
3. Understanding Your Data - df.info()
df.info() is always your FIRST step with any new dataset. It tells you:

How many rows you have
How many columns you have
Which columns have missing data
What type of data each column contains

```python
df.info()
```



Example output from Titanic dataset:
RangeIndex: 891 entries, 0 to 890
Data columns (total 12 columns):
 #   Column       Non-Null Count  Dtype
---  ------       --------------  -----
 0   PassengerId  891 non-null    int64
 1   Survived     891 non-null    int64
 2   Pclass       891 non-null    int64
 3   Name         891 non-null    object
 4   Sex          891 non-null    object
 5   Age          714 non-null    float64   <- 177 missing!
 6   SibSp        891 non-null    int64
 7   Parch        891 non-null    int64
 8   Ticket       891 non-null    object
 9   Fare         891 non-null    float64
 10  Cabin        204 non-null    object    <- 687 missing!
 11  Embarked     889 non-null    object    <- 2 missing
Data types explained:
DtypeMeaningExampleint64Whole numbers1, 2, 3float64Decimal numbers23.5, 7.25objectText/String"male", "John Smith"

How to check missing values directly:
```python
df.isnull().sum()
```

4. Types of Plots
Scatter Plot
Used when you have TWO number columns and want to see if there is any relationship between them.
Rule: Scatter plots work with NUMBERS ONLY!
```python
sns.scatterplot(data=df, x='Age', y='Fare', hue='Sex', size='Pclass')
plt.title('Age vs Fare by Sex and Class')
plt.show()
```
When to use:

Number column vs Number column
To see if 2 things relate to each other
Example: Does Age affect Fare?


Histogram
Shows the distribution of a numerical variable by dividing data into bins and counting observations in each bin.

```python
plt.figure(figsize=(8, 5))
sns.histplot(data=df, x='Age', kde=True, bins=20)
plt.title('Distribution of Age')
plt.xlabel('Age')
plt.ylabel('Count')
plt.show()

```
When to use:

ONE number column only
To see how your data is spread out
Example: How are passenger ages distributed?


KDE Plot
KDE (Kernel Density Estimate) is a smooth curve that shows where most of your data is concentrated. Smoother than histograms and not affected by bin sizes.
```
python
plt.figure(figsize=(8, 5))
sns.kdeplot(data=df, x='Fare', fill=True)
plt.title('Density Plot of Fare')
plt.xlabel('Fare')
plt.ylabel('Density')
plt.show()

```
Simple rule:

ONE column = KDE
High curve = many values there
Low curve = few values there


Regression Plot
Used to visualize the linear relationship (trend) between two variables with a regression line and confidence interval.


```python
plt.figure(figsize=(8, 6))
sns.regplot(data=df, x='Age', y='Fare', scatter_kws={'alpha':0.3})
plt.title('Linear Relationship between Age and Fare')
plt.xlabel('Age')
plt.ylabel('Fare')
plt.show()

```
Reading the regression line:
Line directionMeaningGoes UP ↗️Positive trendGoes DOWN ↘️Negative trendStays FLAT →No relationship
Two functions:
```
sns.regplot - Simple, quick analysis
sns.lmplot - More powerful, can split by groups using hue
```

Box Plot
Excellent for showing the distribution of a numerical variable across different categories. Shows median, quartiles and outliers.

```
python
plt.figure(figsize=(10, 6))
sns.boxplot(data=df, x='Pclass', y='Age', hue='Survived')
plt.title('Age Distribution by Pclass and Survival')
plt.xlabel('Passenger Class')
plt.ylabel('Age')
plt.show()

```
What each part means:
PartMeaningMiddle lineMedian - the middle valueThe boxWhere the middle 50% of data livesWhiskersFull range of normal valuesDots outsideOutliers - unusual values

Violin Plot
Similar to box plots but also shows the probability density of data at different values. Combines Box Plot + KDE in one chart.

```
python
plt.figure(figsize=(10, 6))
sns.violinplot(data=df, x='Pclass', y='Age', hue='Survived')
plt.title('Age Distribution by Pclass and Survival (Violin Plot)')
plt.show()

```
Reading a Violin Plot:

Wide part = many passengers at that age
Narrow part = few passengers at that age
Widest point = most common age in that group

Box PlotViolin PlotShows medianYesYesShows spreadYesYesShows outliersYesYesShows shape of dataNoYes

Count Plot
Shows the count of observations in each category using bars. Essentially a histogram for categorical variables.

```python

sns.countplot(data=df, x='Sex', hue='Survived')
plt.title('Count of Passengers by Sex')
plt.show()

```
Simple rule:

Numbers = Histogram
Categories = Count Plot

When to use:

How many males vs females?
How many in each class?
How many survived vs died?


Pair Plot
Creates a grid of scatter plots for each pair of numerical variables and histograms/KDE plots on the diagonal.

```python
sns.pairplot(df, hue='Survived')
plt.suptitle('Pairwise Relationships of Age, Fare, Pclass, and Survival')
plt.show()

```
What it shows:

Diagonal boxes = KDE of each individual column
Other boxes = Scatter plot between 2 columns
One chart replaces many individual charts!


Heatmap
Turns a correlation matrix into colors so patterns are easy to see at a glance.

```python
plt.figure(figsize=(10, 8))
sns.heatmap(correlation_matrix, annot=True, fmt='.2f', cmap='coolwarm')
plt.title('Correlation Heatmap')
plt.show()

```
Reading colors:
ColorValueMeaningDark RedClose to +1Strong positive relationshipLight PinkAround +0.2Weak positive relationshipWhiteAround 0No relationshipLight BlueAround -0.2Weak negative relationshipDark BlueClose to -1Strong negative relationshipDiagonalAlways 1.0Every column is perfect with itself

5. Correlation and Covariance
Covariance
Covariance tells you if 2 things move in the same direction or opposite directions.
python# Select only number columns
numerical_df = df.select_dtypes(include='number')

# Calculate covariance
cov_matrix = numerical_df.cov()
print(cov_matrix)
Covariance ValueMeaningPositive numberBoth move in same direction ↗️↗️Negative numberThey move in opposite directions ↗️↘️Near zeroNo relationship
Problem with Covariance:

It is scale dependent
Changing units changes the value
Hard to compare different relationships


Correlation
Correlation fixes the covariance problem by putting everything on a scale from -1 to +1.
python# Calculate correlation
correlation_matrix = numerical_df.corr()
correlation_matrix
The Correlation Scale:
-1          -0.5          0          +0.5          +1
|_____________|_____________|_____________|_____________|
Strong      Weak         None         Weak        Strong
Negative   Negative               Positive     Positive
Key correlations from Titanic dataset:
RelationshipValueMeaningSurvived vs Pclass-0.338Poorer class = less survivalSurvived vs Fare+0.257Higher fare = more survivalPclass vs Fare-0.550Higher class number = cheaper farePclass vs Age-0.369Higher class number = younger passengersSibSp vs Parch+0.415Families travelled together

6. Correlation Does NOT Mean Causation
Correlation simply tells you how strongly two variables move together. It does NOT tell you why they move together.
Famous example:

Ice cream sales go UP when drowning cases go UP
Does ice cream cause drowning? NO!
Hot weather causes BOTH!

Always ask WHY before concluding X causes Y!
There could be:

A hidden third factor causing both
A pure coincidence
A non-causal relationship


Quick Reference - Which Chart to Use?
QuestionChartHow is ONE number column distributed?Histogram or KDERelationship between TWO number columns?Scatter PlotGeneral trend between TWO number columns?Regression PlotDistribution across categories?Box Plot or Violin PlotHow many in each category?Count PlotAll relationships at once?Pair PlotCorrelation between all columns?Heatmap

Dataset Used
Titanic Dataset - 891 passengers, 12 columns
ColumnDescriptionPassengerIdUnique ID for each passengerSurvived0 = No, 1 = YesPclassTicket class - 1 = First, 2 = Second, 3 = ThirdNamePassenger nameSexMale or FemaleAgePassenger ageSibSpNumber of siblings/spouses on boardParchNumber of parents/children on boardTicketTicket numberFareTicket priceCabinCabin numberEmbarkedPort of embarkation - C, Q, S

Libraries Used
```
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
pythonimport pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

Made with curiosity and a lot of questions 😊
