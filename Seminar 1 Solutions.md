<div style="text-align: center; font-family: 'Times New Roman', Times, serif; font-size: 16pt; font-weight: bold">
  
## Seminar#1<br>Tasks solutions 
</div>

<div style="font-family: 'Times New Roman', Times, serif; font-size: 12pt; text-align: left">

---

## Task 1: Determine the dataset's dimensionality (number of rows and columns).

### Solution:
```python
df.shape
```

### Interpretation:
The `shape` attribute is a fundamental property of a Pandas DataFrame that returns a tuple `(rows, columns)`. It provides an immediate overview of the dataset's size, which is crucial for understanding the scale of the data and planning further analysis steps.

---

## Task 2: Print the names of all columns in the dataset.

### Solution:
```python
df.columns.tolist()
```

### Interpretation:
Accessing `df.columns` returns an Index object containing the column names. Using `.tolist()` converts this Index to a standard Python list, making it easy to print or manipulate the column names programmatically. This is essential for understanding the features available in the dataset.

---

## Task 3: Calculate the total number of approved loans (action == 1).

### Solution:
```python
approved_loans = df[df['action'] == 1]
len(approved_loans)
```

### Interpretation:
This solution uses boolean indexing to filter the DataFrame. `df['action'] == 1` creates a boolean mask, selecting only rows where the 'action' column equals 1. The `len()` function then counts the resulting rows. An alternative, more concise approach would be `len(df[df['action'] == 1])` or `df[df['action'] == 1].shape[0]`.

---

## Task 4: Find the minimum value of the 'loanamt' variable (loan amount).

### Solution:
```python
df['loanamt'].min()
```

### Interpretation:
The `.min()` method efficiently calculates the minimum value in a specific Series (column). This is useful for identifying the smallest loan amount in the dataset, which helps understand the range of loan sizes.

---

## Task 5: Calculate the average loan amount applied for.

### Solution:
```python
df['loanamt'].mean()
```

### Interpretation:
The `.mean()` method computes the arithmetic mean of the values in the 'loanamt' column. This central tendency measure gives an idea of the typical loan application amount.

---

## Task 6: Find the quartiles (25%, 50%, 75%) for the loan amount.

### Solution:
```python
df['loanamt'].quantile([0.25, 0.5, 0.75])
```

### Interpretation:
The `.quantile()` method calculates specified quantiles. Passing the list `[0.25, 0.5, 0.75]` returns the 25th percentile (Q1), 50th percentile (median/Q2), and 75th percentile (Q3). This provides insight into the distribution of loan amounts beyond just the mean.

---

## Task 7: Find the correlation between the loan amount (loanamt) and the applicant's income (appinc).

### Solution:
```python
df[['loanamt', 'appinc']].corr()
```

### Interpretation:
The `.corr()` method computes the Pearson correlation coefficient matrix for the selected columns. Correlation values range from -1 to 1, indicating the strength and direction of a linear relationship. This helps determine if higher applicant income is associated with larger loan amounts.

---

## Task 8: Create a frequency table for the 'married' variable.

### Solution:
```python
df['married'].value_counts()
```

### Interpretation:
The `.value_counts()` method generates a frequency table, showing the count of unique values in the 'married' column. This is ideal for categorical variables to quickly see how many observations belong to each category (e.g., married vs. unmarried).

---

## Task 9: Find for each employment type (occ).

### Solution:
```python
df.groupby('occ')['loanamt'].mean()
```

### Interpretation:
This solution uses the `.groupby()` method to split the data by the 'occ' (occupation) column. Then, the `.mean()` function is applied specifically to the 'loanamt' column for each group. This calculates the average loan amount separately for each employment type, revealing potential patterns.

---

## Task 10: Calculate the interquartile range (IQR) for the loan amount.

### Solution:
```python
Q1 = df['loanamt'].quantile(0.25)
Q3 = df['loanamt'].quantile(0.75)
IQR = Q3 - Q1
IQR
```

### Interpretation:
The Interquartile Range (IQR) is a measure of statistical dispersion, representing the middle 50% of the data. It is calculated as the difference between the 75th percentile (Q3) and the 25th percentile (Q1). The IQR is robust to outliers and helps understand the spread of the central portion of the loan amount distribution.