<div style="text-align: center; font-family: 'Times New Roman', Times, serif; font-size: 16pt; font-weight: bold">
  
# Cheat Sheet for Data Analysis #7<br>Financial Data Analysis with APIs
</div>

<div style="font-family: 'Times New Roman', Times, serif; font-size: 12pt; text-align: left">

## Basic Setup and Data Loading

### Import essential libraries
```python
import yfinance as yf
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```
*Meaning:* Imports necessary libraries for accessing financial data and visualization

### Download stock data using yfinance
```python
# Download historical data for a specific ticker
stock = yf.Ticker("AAPL")
data = stock.history(start="2020-01-01", end="2023-01-01", repair=True)
```
*Meaning:* 
- `Ticker("AAPL")` creates an object for Apple stock
- `history()` downloads historical price data
- `start/end` specify date range (YYYY-MM-DD format)
- `repair=True` fixes potential data inconsistencies

### View basic dataset information
```python
print("Data shape:", data.shape)
print("\nAvailable columns:", data.columns.tolist())
print("\nDate range:", data.index.min(), "to", data.index.max())
data.head()
```
*Meaning:* Displays dimensions, column names, date range, and first few rows for quick inspection

---

## Working with Financial Data

### Extract closing prices
```python
prices = data['Close']
```
*Meaning:* Extracts the closing price series; note that `history()` returns adjusted close by default

### Check for missing values
```python
print("Missing values per column:")
print(data.isnull().sum())
```
*Meaning:* Identifies any gaps in the time series data that need to be addressed

### Resample to different time periods
```python
# Weekly data
weekly_data = data.resample('W').last()

# Monthly data
monthly_data = data.resample('M').last()
```
*Meaning:* Converts daily data to weekly or monthly frequency using the last observation of each period

---

## Return Calculations

### Calculate simple returns
```python
data['Simple_Return'] = prices.pct_change()
data = data.dropna()  # Remove first row with NaN
```
*Meaning:* 
- `.pct_change()` computes percentage change from previous period
- Simple return = (P_t - P_{t-1}) / P_{t-1}
- First value will be NaN (no previous period), hence `dropna()`

### Calculate log returns
```python
data['Log_Return'] = np.log(prices / prices.shift(1))
data = data.dropna()
```
*Meaning:* 
- Log return = ln(P_t / P_{t-1})
- Log returns are time-additive and often assumed to be normally distributed
- Generally preferred for statistical modeling

### Compare simple vs. log returns
```python
print(f"Mean Simple Return: {data['Simple_Return'].mean():.6f}")
print(f"Mean Log Return: {data['Log_Return'].mean():.6f}")
print(f"Std Dev (Daily): {data['Log_Return'].std():.6f}")
print(f"Volatility (Annual): {data['Log_Return'].std() * np.sqrt(252):.6f}")
```
*Meaning:* 
- Compares key statistics of both return types
- Annual volatility = daily volatility × √252 (trading days in a year)

---

## Financial Data Visualization

### Plot price time series
```python
plt.figure(figsize=(10, 6))
plt.plot(data.index, data['Close'])
plt.title('AAPL Stock Price (2020-2023)')
plt.xlabel('Date')
plt.ylabel('Price (USD)')
plt.grid(True, alpha=0.3)
plt.show()
```
*Meaning:* Visualizes the historical price movement; essential for identifying trends and patterns

### Plot returns distribution
```python
plt.figure(figsize=(10, 6))
plt.hist(data['Log_Return'], bins=50, color='skyblue', edgecolor='black')
plt.title('Daily Returns Distribution')
plt.xlabel('Log Return')
plt.ylabel('Frequency')
plt.grid(True, alpha=0.3, axis='y')
plt.axvline(x=data['Log_Return'].mean(), color='red', linestyle='--', label='Mean')
plt.legend()
plt.show()
```
*Meaning:* Shows the distribution of returns; helps identify skewness, kurtosis, and outliers

### Compare simple vs. log returns
```python
fig, axes = plt.subplots(2, 2, figsize=(12, 8))

# Time series comparison
axes[0, 0].plot(data.index, data['Simple_Return'], label='Simple', alpha=0.7)
axes[0, 0].plot(data.index, data['Log_Return'], label='Log', alpha=0.7, linestyle='--')
axes[0, 0].set_title('Time Series: Simple vs Log Returns')
axes[0, 0].legend()
axes[0, 0].grid(True, alpha=0.3)
axes[0, 0].axhline(y=0, color='black', linestyle='-', linewidth=0.5)

# Scatter plot
axes[0, 1].scatter(data['Simple_Return'], data['Log_Return'], alpha=0.5, s=10)
axes[0, 1].plot([-0.1, 0.1], [-0.1, 0.1], 'r--', label='45° Line')
axes[0, 1].set_title('Scatter: Simple vs Log Returns')
axes[0, 1].set_xlabel('Simple Return')
axes[0, 1].set_ylabel('Log Return')
axes[0, 1].legend()
axes[0, 1].grid(True, alpha=0.3)

# Distribution comparison
axes[1, 0].hist(data['Simple_Return'], bins=40, alpha=0.6, color='blue', label='Simple')
axes[1, 0].hist(data['Log_Return'], bins=40, alpha=0.6, color='orange', label='Log')
axes[1, 0].set_title('Distribution Comparison')
axes[1, 0].set_xlabel('Return')
axes[1, 0].set_ylabel('Frequency')
axes[1, 0].legend()
axes[1, 0].grid(True, alpha=0.3, axis='y')

# Difference plot
difference = data['Simple_Return'] - data['Log_Return']
axes[1, 1].plot(data.index, difference, color='green', linewidth=1)
axes[1, 1].set_title('Difference: Simple - Log')
axes[1, 1].set_xlabel('Date')
axes[1, 1].set_ylabel('Difference')
axes[1, 1].grid(True, alpha=0.3)
axes[1, 1].axhline(y=0, color='black', linestyle='-', linewidth=0.5)

plt.tight_layout()
plt.show()
```
*Meaning:* Comprehensive comparison of simple and log returns across multiple dimensions

### Calculate volatility
```python
# 30-day rolling volatility
data['Volatility'] = data['Log_Return'].rolling(window=30).std() * np.sqrt(252)

plt.figure(figsize=(10, 6))
plt.plot(data.index, data['Volatility'])
plt.title('30-Day Rolling Volatility (Annualized)')
plt.xlabel('Date')
plt.ylabel('Volatility')
plt.grid(True, alpha=0.3)
plt.show()
```
*Meaning:* Measures changing risk over time; higher volatility indicates greater price fluctuations

---

## Practical Tips for Financial Data Analysis

1. **Always verify data quality:**
   - Check for missing values with `data.isnull().sum()`
   - Use `repair=True` parameter to fix common data issues
   - Verify date ranges match your expectations

2. **Choose the right return type:**
   - Use **simple returns** for single-period calculations and portfolio returns
   - Use **log returns** for multi-period calculations, statistical modeling, and when returns are assumed to be normally distributed

3. **Annualize volatility correctly:**
   - Daily volatility × √252 (trading days)
   - Weekly volatility × √52
   - Monthly volatility × √12

4. **Compare multiple assets:**
   ```python
   tickers = ["AAPL", "MSFT", "GOOG"]
   data = yf.download(tickers, start="2020-01-01")["Adj Close"]
   returns = data.pct_change().dropna()
   ```
   *Meaning:* Download and compare multiple assets simultaneously

5. **Normalize prices for comparison:**
   ```python
   normalized = data['Close'] / data['Close'].iloc[0] * 100
   ```
   *Meaning:* Shows percentage change from starting point, enabling direct comparison of different-priced assets

6. **Create efficient visualizations:**
   - Use `figsize=(10, 6)` for standard plots
   - Add grid lines with `alpha=0.3` for readability
   - Always label axes and add titles
   - Use consistent color schemes

7. **Understand the limitations:**
   - Free APIs like yfinance have rate limits
   - Historical data may contain errors
   - Past performance ≠ future results (obviously!)

Remember: Financial data analysis requires both technical skills and domain knowledge. Always verify your results with multiple approaches and consider the broader economic context when interpreting your findings.
