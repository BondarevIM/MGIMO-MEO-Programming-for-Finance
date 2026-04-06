<div style="text-align: center; font-family: 'Times New Roman', Times, serif; font-size: 16pt; font-weight: bold">
  
# Seminar 3 Solutions
</div>

<div style="font-family: 'Times New Roman', Times, serif; font-size: 12pt; text-align: left">

## Task 1: Download stock prices data for Google (GOOGL) from January 1, 2020 to January 1, 2023. Which method is used to download historical price data for a specific ticker object?

```python
stock = yf.Ticker("GOOGL")
data = stock.history(start="2020-01-01", end="2023-01-01", repair=True)
```

**Correct answer: .history()**

### Commentary
This solution demonstrates the standard workflow for accessing financial data using the yfinance library. First, a Ticker object is created with `yf.Ticker("GOOGL")`, which serves as an interface to Google's stock data. The `.history()` method is then called on this object to download historical price data. The parameters used are:
- `start` and `end`: Define the date range for the data (YYYY-MM-DD format)
- `repair=True`: A valuable parameter that attempts to fix common data inconsistencies, such as missing values or irregular trading days

The `.history()` method returns a DataFrame containing Open, High, Low, Close, Volume, and other relevant financial data. By default, the 'Close' column represents adjusted closing prices that account for splits and dividends, making it suitable for return calculations without additional adjustments. This approach is preferred over the alternative `yf.download()` method when working with a single ticker, as it provides more flexibility and access to additional company information through the Ticker object.

## Task 2: From May, 2020 to September, 2021 the price for Google stocks was mainly...

```python
plt.figure(figsize=(10, 6))
plt.plot(data.index, data['Close'])
plt.title('GOOGL Stock Price (2020-2023)')
plt.xlabel('Date')
plt.ylabel('Price (USD)')
plt.grid(True, alpha=0.3)
plt.show()
```

**Correct answer: growing**

### Commentary
The time series plot visualizes Google's stock price trajectory from 2020 to 2023. To determine the trend between May 2020 and September 2021, we need to examine this specific segment of the chart. The visualization technique used (line plot with grid lines) effectively highlights the trend direction while minimizing visual noise. When analyzing financial time series, it's important to distinguish between short-term fluctuations and longer-term trends, which this plot clearly demonstrates for the specified period.

## Task 3: When was the peak price achieved in the said time period (January 1, 2020 to January 1, 2023)?

### Commentary
Based on the visualization from Task 2, the peak price for Google stock during the 2020-2023 period occurred in December 2021. This timing aligns with broader market patterns where many technology stocks reached their highest valuations at the end of 2021, before facing headwinds in 2022 from rising interest rates, inflation concerns, and changing pandemic dynamics. The specific peak likely occurred in late November or early December 2021.

## Task 4: What was the approximate mean simple return (January 1, 2020 to January 1, 2023)? Round to 4 decimal places.

```python
prices = data['Close']
data['Simple_Return'] = prices.pct_change()
data = data.dropna()  # Remove first row with NaN
print(f"Mean Simple Return: {data['Simple_Return'].mean():.4f}")
```

**Correct answer: 0.0006**

### Commentary
This solution calculates the mean simple return for Google stock over the 2020-2023 period. Simple returns are calculated using the percentage change formula: (P_t - P_{t-1}) / P_{t-1}, implemented in pandas via the `.pct_change()` method. The first row contains NaN because there's no previous period to compare with, hence the `dropna()` call. The mean daily simple return of approximately 0.0006 (or 0.06%) indicates that, on average, Google's stock price increased by this small percentage each trading day. When annualized (0.06% × 252 trading days), this represents a roughly 15.12% annual return, which is consistent with Google's strong performance during this period. Simple returns are intuitive to understand and calculate, making them useful for single-period analysis and portfolio return calculations, though log returns are often preferred for multi-period statistical modeling due to their time-additive properties.

## Task 5: When was the 30-Day Rolling Volatility (Annualized) of log return the highest for the said time period (January 1, 2020 to January 1, 2023)?

```python
data['Log_Return'] = np.log(prices / prices.shift(1))
data = data.dropna()
data['Volatility'] = data['Log_Return'].rolling(window=30).std() * np.sqrt(252)
plt.figure(figsize=(10, 6))
plt.plot(data.index, data['Volatility'])
plt.title('30-Day Rolling Volatility (Annualized)')
plt.xlabel('Date')
plt.ylabel('Volatility')
plt.grid(True, alpha=0.3)
plt.show()
```

**Correct answer: April 2020**

### Commentary
This solution calculates and visualizes the 30-day rolling volatility of Google stock, annualized for standardization. Volatility, measured as the standard deviation of returns, is a key risk metric in finance. The rolling window approach (30 days) captures changing risk levels over time rather than providing a single static measure. The multiplication by √252 converts daily volatility to annualized terms (assuming 252 trading days in a year), allowing for consistent comparison across different time periods. The highest volatility peak occurs in April 2020, which corresponds to the initial market turmoil following the global spread of COVID-19. During this period, financial markets experienced extreme volatility as investors reacted to unprecedented economic shutdowns, government stimulus measures, and uncertainty about the pandemic's duration. The visualization clearly shows this volatility spike, which reached levels significantly higher than the subsequent market fluctuations. Understanding these volatility patterns is essential for risk management, options pricing, and portfolio construction.

## Task 6: Based on the stock prices data for Nike from January 1, 2022 to January 1, 2026, what was the peak price approximately equal to?

```python
stock = yf.Ticker("NKE")
data1 = stock.history(start="2022-01-01", end="2026-01-01", repair=True)
plt.figure(figsize=(10, 6))
plt.plot(data1.index, data1['Close'])
plt.title('NKE Stock Price (2022-2025)')
plt.xlabel('Date')
plt.ylabel('Price (USD)')
plt.grid(True, alpha=0.3)
plt.show()
print(data1['Close'].max())
```

**Correct answer: 150$**

### Commentary
The solution downloads Nike's stock data for the specified period and identifies the peak price through both visualization and direct calculation. The line plot provides a visual representation of Nike's price trajectory, while the `data1['Close'].max()` command programmatically determines the highest closing price. Nike's stock reached its peak around $150 during this period, which occurred in late 2021/early 2022 before experiencing a decline. This peak likely coincided with strong post-pandemic consumer demand for athletic apparel, successful product launches, and positive earnings reports. The ability to identify peak prices is crucial for technical analysis, as these levels often become psychological resistance points that influence future price movements. When analyzing historical peaks, it's important to consider the broader market context and company-specific factors that contributed to the high price, as these insights can inform future investment decisions and risk assessments.

## Task 7: For Nike, calculate the mean simple return (from January 1, 2022 to January 1, 2026). Round to 6 decimal places.

## Task 8: For Nike, calculate the daily standard deviation for simple returns. (from January 1, 2022 to January 1, 2026). Round to 6 decimal places.

## Task 9: For Nike, calculate the annual volatility (from January 1, 2022 to January 1, 2026) for log returns. Round to 6 decimal places.

```python
prices1 = data1['Close']
data1['Simple_Return'] = prices1.pct_change()
data1 = data1.dropna()  # Remove first row with NaN
data1['Log_Return'] = np.log(prices1 / prices1.shift(1))
data1 = data1.dropna()
print(f"NKE Mean Simple Return: {data1['Simple_Return'].mean():.6f}")
print(f"NKE Std Dev (Daily): {data1['Simple_Return'].std():.6f}")
print(f"NKE Volatility (Annual): {data1['Log_Return'].std() * np.sqrt(252):.6f}")
```

**Correct answers:**
- Task 7: -0.000617
- Task 8: 0.023199
- Task 9: 0.364036

### Commentary
These three tasks collectively analyze Nike's return characteristics using both simple and log returns. The negative mean simple return (-0.000617) indicates that, on average, Nike's stock price decreased slightly each trading day during this period, reflecting an overall downward trend consistent with the peak identified in Task 6 followed by a decline. The daily standard deviation (0.023199 or 2.32%) measures the typical daily price fluctuation, representing Nike's day-to-day volatility. The annualized volatility (36.40%) converts this daily measure to an annual scale using √252, providing a standardized risk metric comparable across different assets and time periods. This relatively high volatility suggests Nike experienced significant price swings during this period, possibly due to changing consumer preferences, supply chain challenges, or broader market volatility. The slight difference between simple return standard deviation and log return volatility (used for annualization) highlights why log returns are preferred for volatility calculations - they better capture the compounding nature of financial returns and maintain time-additivity properties essential for multi-period risk modeling.

## Task 10: For Amazon, calculate the mean log return (from May 1, 2023 to May 1, 2025). Round to 5 decimal places.

```python
stock = yf.Ticker("AMZN")
data2 = stock.history(start="2023-05-01", end="2025-05-01", repair=True)
prices2 = data2['Close']
data2['Log_Return'] = np.log(prices2 / prices2.shift(1))
data2 = data2.dropna()
print(f"AMZN Mean Log Return: {data2['Log_Return'].mean():.5f}")
```

**Correct answer: 0.00118**

### Commentary
This solution calculates Amazon's mean log return over a specific two-year period. Log returns, calculated as ln(P_t/P_{t-1}), have several advantages over simple returns for statistical analysis: they are time-additive (the sum of log returns equals the log return over the entire period), symmetric (up and down movements are treated equally), and often better approximate a normal distribution - a common assumption in financial modeling. Amazon's positive mean log return of 0.00118 (0.118% per day) indicates a generally upward price trend during this period. When annualized (0.00118 × 252 ≈ 0.297 or 29.7%), this represents a strong expected return, reflecting Amazon's growth in cloud computing (AWS), e-commerce recovery, and cost-cutting measures during this timeframe. The use of log returns for this calculation is particularly appropriate as it provides a more accurate measure of compounded growth over the multi-year period compared to simple returns, especially when returns exhibit significant variation.