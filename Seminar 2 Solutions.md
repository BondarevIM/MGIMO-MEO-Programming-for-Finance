<div style="text-align: center; font-family: 'Times New Roman', Times, serif; font-size: 16pt; font-weight: bold">
  
# Seminar 2 Solutions
</div>

<div style="font-family: 'Times New Roman', Times, serif; font-size: 12pt; text-align: left">

## Task 1: Calculate the correlation between 'loanamt' (loan amount) and 'appinc' (applicant's income). What is the correlation (round to 2 decimal places)?

```python
correlation = df['loanamt'].corr(df['appinc'])
correlation
```

**Correct answer: 0.43**

### Commentary
This solution calculates the Pearson correlation coefficient between loan amount and applicant income. The `.corr()` method in pandas computes the standard correlation coefficient, which measures the strength and direction of the linear relationship between two numerical variables. A correlation of 0.43 indicates a moderate positive relationship - as applicant income increases, loan amount tends to increase as well, but not perfectly. This makes logical sense in lending contexts, as higher-income applicants typically qualify for larger loans. Note that correlation does not imply causation - we cannot say that higher income causes larger loans, only that they tend to occur together.

## Task 2: Calculate the average 'loanamt' for each 'occ' category (occupation). Which occupation has the highest average?

```python
mean_loan_occ = df.groupby('occ')['loanamt'].mean()
highest_mean_occ = mean_loan_occ.idxmax()
highest_mean_occ
```

**Correct answer: 1**

### Commentary
This solution uses the `groupby()` method to calculate the mean loan amount for each occupation category. The `groupby()` function splits the data into groups based on the 'occ' (occupation) column, then applies the `mean()` function to the 'loanamt' column within each group. The resulting Series contains occupation codes as indices and their corresponding average loan amounts as values. The `idxmax()` method then identifies which occupation code has the highest average loan amount. Occupation code 1 having the highest average suggests that applicants in this occupation category tend to receive larger loans, possibly due to higher perceived creditworthiness, income levels, or specific loan purposes associated with this occupation.

## Task 3: Find the mode (most frequent value) for the variable 'dep' (number of dependents). Which value is the mode?

```python
mode_dep = df['dep'].mode()[0]
mode_dep
```

**Correct answer: 0.0**

### Commentary
The mode is the most frequently occurring value in a dataset. In pandas, the `mode()` method returns a Series containing the most frequent value(s). Since the dataset likely has a single mode, we use `[0]` to extract the first (and in this case, only) value from the resulting Series. The mode being 0.0 indicates that the most common number of dependents among loan applicants is zero - meaning most applicants have no dependents. This is a typical finding in loan datasets, as younger applicants or those without families often seek loans for education, vehicles, or other personal expenses. Understanding the mode helps identify the most typical case in categorical or discrete numerical data.

## Task 4: Visualize the distribution of the target variable 'action' (application status: 1 - approved, 3 - rejected) using a bar chart. Which application status occurs more frequently?

```python
action_counts = df['action'].value_counts()
plt.figure(figsize=(6, 4))
sns.countplot(x='action', data=df)
plt.title('Distribution of Loan Application Outcomes')
plt.show()
most_common_action = action_counts.idxmax()
most_common_action
```

**Correct answer: 1**

### Commentary
This solution first uses `value_counts()` to count occurrences of each application status, then creates a bar chart using seaborn's `countplot()`. The `value_counts()` method efficiently tabulates how many times each value appears in the 'action' column. The resulting visualization clearly shows the frequency distribution of loan outcomes. The fact that action code 1 (approved) occurs more frequently than code 3 (rejected) suggests that the lending institution has a relatively high approval rate.

## Task 5: Plot a histogram for the variable 'appinc' (applicant's income). What conclusion can be drawn about the income distribution?

```python
plt.figure(figsize=(8, 6))
plt.hist(df['appinc'], bins=50, edgecolor='k')
plt.title('Distribution of Applicant Income')
plt.xlabel('Income (in thousands)')
plt.ylabel('Frequency')
plt.show()
```

**Correct answer: right-skewed**

### Commentary
The histogram visualizes the distribution of applicant income across 50 bins. The right-skewed (positively skewed) distribution means most applicants have lower incomes, with a long tail extending toward higher incomes. This is typical for income distributions in most populations due to the presence of high-income outliers. In a right-skewed distribution, the mean would be greater than the median, indicating that a small number of high-income applicants pull the average up. This skewness has important implications for analysis - many statistical techniques assume normality, so transformations (like log transformations) might be needed before certain analyses. Understanding income distribution helps in assessing credit risk, as income is typically a key factor in loan approval decisions.

## Task 6: Visualize the relationship between 'yjob' (length of service) and 'loanamt' (loan amount) using a scatter plot. Is the relationship visually linear? (Answer: yes/no, assume from the graph)

```python
plt.figure(figsize=(8, 6))
plt.scatter(df['yjob'], df['loanamt'], alpha=0.6)
plt.title('Years in Job vs Loan Amount')
plt.xlabel('Years in Current Job')
plt.ylabel('Loan Amount')
plt.show()
```

**Correct answer: no**

### Commentary
The scatter plot visualizes the relationship between job tenure and loan amount. The absence of a clear linear pattern indicates that the relationship is not linear. This could manifest in several ways: the points might form a curved pattern, show no discernible pattern at all, or display different relationships in different segments of the data. The non-linear relationship suggests that if we were building a predictive model, we might need to consider polynomial terms, interaction effects, or non-linear modeling techniques rather than a simple linear regression.

## Task 7: Plot a bar plot of the average 'loanamt' over 'occ'. Which 'occ' category is at the top? (See the results from previous tasks)

```python
occ_means = df.groupby('occ')['loanamt'].mean()
plt.figure(figsize=(10, 6))
occ_means.plot(kind='bar')
plt.title('Average Loan Amount by Occupation')
plt.xlabel('Occupation Code')
plt.ylabel('Average Loan Amount')
plt.xticks(rotation=45)
plt.show()
```

**Correct answer: 1**

### Commentary
This bar plot visualizes the results calculated in Task 2, showing the average loan amount for each occupation category. Occupation code 1 appears at the top of the chart, confirming it has the highest average loan amount. The visualization makes it easy to compare across categories and identify outliers. In lending analysis, this pattern might suggest that occupation code 1 represents professions with higher earning potential, more stable employment, or specific loan purposes requiring larger amounts. When interpreting such results, it's important to consider whether the differences are statistically significant and whether other variables (like income within each occupation) might explain the variation in loan amounts.

## Task 8: Visualize the correlation between 'appinc' (applicant's income) and 'atotinc' (applicant's final income) using a scatter plot. What is the visual relationship between these variables? (Answer: strong positive, weak, or none)

```python
plt.figure(figsize=(8, 6))
plt.scatter(df['appinc'], df['atotinc'], alpha=0.6)
plt.title('Applicant Income vs Total Income')
plt.xlabel('Applicant Income (appinc)')
plt.ylabel('Total Income (atotinc)')
plt.show()
```

**Correct answer: positive correlation**

### Commentary
The scatter plot reveals a strong positive correlation between applicant income (appinc) and total income (atotinc). This suggests these variables are likely measuring similar or related concepts - possibly appinc represents primary income while atotinc includes additional income sources. In many loan datasets, these variables might be nearly identical with minor variations, explaining the strong relationship. The high correlation indicates potential redundancy in the dataset, which could lead to multicollinearity issues if both variables are used in the same regression model. When variables are highly correlated, analysts often choose to keep only one to avoid statistical complications, unless there's a specific theoretical reason to include both.

## Task 9: Plot a box plot to compare the 'obrat' (Obligations-to-Income Ratio) for approved ('action' == 1) and rejected ('action' == 3) applications. Which group has a higher median 'obrat'?

```python
plt.figure(figsize=(8, 6))
sns.boxplot(x='action', y='obrat', data=df)
plt.title('Obligations-to-Income Ratio by Loan Action')
plt.xlabel('Action (1: Approved, 3: Rejected)')
plt.ylabel('Obligations-to-Income Ratio')
plt.show()
```

**Correct answer: 3**

### Commentary
The box plot compares the distribution of obligations-to-income ratio (obrat) between approved and rejected loan applications. The median obrat is higher for rejected applications (action code 3), which makes logical sense - a higher obligations-to-income ratio indicates that a larger portion of the applicant's income would go toward debt payments, representing higher credit risk. The box plot also reveals the spread and potential outliers within each group, providing a more complete picture than just the median.

## Task 10: Plot a stacked bar chart showing the distribution of marital status ('married': 0 - single, 1 - married) by gender ('male': 0 - female, 1 - male). Which segment (married/single) predominates among men?

```python
# Create a pivot table
marital_gender_table = pd.crosstab(df['male'], df['married'])
print(marital_gender_table)

plt.figure(figsize=(8, 6))
marital_gender_table.plot(kind='bar', stacked=True)
plt.title('Marital Status by Gender')
plt.xlabel('Gender (0: Female, 1: Male)')
plt.ylabel('Count')
plt.legend(title='Married (0: No, 1: Yes)', labels=['Not Married', 'Married'])
plt.xticks(rotation=0)
plt.show()
```

**Correct answer: married**

### Commentary
The stacked bar chart visualizes the relationship between gender and marital status using a contingency table created with `pd.crosstab()`. Among men (gender code 1), the "married" segment predominates, indicating that married men outnumber single men in the dataset. This could reflect demographic patterns in the population from which the sample was drawn, or potentially indicate that married men are more likely to apply for loans (perhaps for family-related expenses).
