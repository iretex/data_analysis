
## Bitcoin Historical Data Analysis

### Insight 1: Daily Price Range

- **Measure**: Daily Price Range
- **Definition**: `Daily Range = 'Bitcoin Prices'[High] - 'Bitcoin Prices'[Low]`
- **Visual**: Line chart showing the daily price range over time.

### Insight 2: Average Daily Volume Trend

- **Measure**: Average Daily Volume
- **Definition**: `Average Daily Volume = AVERAGE('Bitcoin Prices'[Volume (BTC)])`
- **Visual**: Line chart with 'Timestamp' on the x-axis and 'Average Daily Volume' on the y-axis.

### Insight 3: Daily Price Change Percentage

- **Measure**: Daily Price Change Percentage
- **Definition**: `Price Change (%) = DIVIDE('Bitcoin Prices'[Close] - 'Bitcoin Prices'[Open], 'Bitcoin Prices'[Open])`
- **Visual**: Line chart displaying the daily price change percentage.

### Insight 4: Weekly Moving Averages

- **Measures**:
  - Weekly Simple Moving Average (SMA): `SMA (Weekly) = AVERAGEX(FILTER('Bitcoin Prices', 'Bitcoin Prices'[Timestamp] >= EARLIER('Bitcoin Prices'[Timestamp]) - 6), 'Bitcoin Prices'[Close])`
  - Weekly Exponential Moving Average (EMA): `EMA (Weekly) = EXPONENTIALAVG('Bitcoin Prices'[Close], 7)`
- **Visual**: Line chart with 'Timestamp' on the x-axis and both SMA (Weekly) and EMA (Weekly) on the y-axis.

### Insight 5: High-Volume Days

- **Measure**: High-Volume Days
- **Definition**: `High Volume Days = COUNTROWS(FILTER('Bitcoin Prices', 'Bitcoin Prices'[Volume (BTC)] > [Threshold]))`
- **Visual**: Bar chart with 'Timestamp' on the x-axis and 'High Volume Days' on the y-axis, where 'Threshold' is a user-defined value.

### Insight 6: Largest Daily Price Gains and Losses

- **Measures**:
  - Largest Daily Gain: `Largest Gain = MAXX('Bitcoin Prices', 'Bitcoin Prices'[High] - 'Bitcoin Prices'[Low])`
  - Largest Daily Loss: `Largest Loss = MINX('Bitcoin Prices', 'Bitcoin Prices'[High] - 'Bitcoin Prices'[Low])`
- **Visual**: Card visuals displaying the largest daily gain and loss.

### Insight 7: Weekly Price Volatility

- **Measure**: Weekly Price Volatility
- **Definition**: `Weekly Volatility = STDEV.P('Bitcoin Prices'[Close])`
- **Visual**: Line chart with 'Timestamp' on the x-axis and 'Weekly Volatility' on the y-axis.

### Insight 8: Monthly Price Trend

- **Measure**: Monthly Average Price
- **Definition**: `Monthly Average Price = AVERAGEX(FILTER('Bitcoin Prices', MONTH('Bitcoin Prices'[Timestamp]) = MONTH([Selected Date])), 'Bitcoin Prices'[Close])`
- **Visual**: Line chart with 'Timestamp' (grouped by month) on the x-axis and 'Monthly Average Price' on the y-axis. Use a slicer to select a specific month.


## Advnaced Insights with Pyhon Scripting Visual

### Python Visual Insight 1: Time-Series Plot

- **Description**: This Python script creates a time-series plot of Bitcoin's close prices over time using the provided 'Timestamp' and 'Close' data fields. The 'Timestamp' column is converted to Unix timestamps for plotting.

- **Code**:
```python
import matplotlib.pyplot as plt
import pandas as pd

# Get the data from Power BI
data = dataset[['Timestamp', 'Close']]

# Convert the 'Timestamp' column to a Unix timestamp
data['Timestamp'] = pd.to_datetime(data['Timestamp'], format='%m/%d/%Y %I:%M:%S %p')
data['UnixTimestamp'] = (data['Timestamp'] - pd.Timestamp("1970-01-01")) // pd.Timedelta('1s')

# Create the time-series plot
plt.figure(figsize=(10, 6))
plt.plot(data['UnixTimestamp'], data['Close'], label='Bitcoin Close Price', color='blue')
plt.xlabel('Unix Timestamp')
plt.ylabel('Price (USD)')
plt.title('Bitcoin Price Time Series')
plt.legend()
plt.grid(True)

# Show the plot
plt.show()
```

### Python Visual Insight 2: High-Volume Days Plot

- **Description**: This Python script creates a bar chart to visualize high-volume days based on a user-defined threshold. It uses the 'Timestamp' and 'Volume (BTC)' data fields and filters days with volumes exceeding the threshold.

- **Code**:
```python
import matplotlib.pyplot as plt
import pandas as pd

# Get the data from Power BI
data = dataset[['Timestamp', 'Volume (BTC)']]

# Filter high-volume days
high_volume_data = data[data['Volume (BTC)'] > threshold]

# Convert the 'Timestamp' column to a datetime object
high_volume_data['Timestamp'] = pd.to_datetime(high_volume_data['Timestamp'], format='%m/%d/%Y %I:%M:%S %p')

# Create a bar chart for high-volume days
plt.figure(figsize=(10, 6))
plt.bar(high_volume_data['Timestamp'], high_volume_data['Volume (BTC)'], color='green')
plt.xlabel('Timestamp')
plt.ylabel('Volume (BTC)')
plt.title('High-Volume Days')
plt.xticks(rotation=45)
plt.grid(True)

# Show the plot
plt.show()
```

### Python Visual Insight 3: Price Change Distribution

- **Description**: This Python script creates a histogram to visualize the distribution of daily price change percentages using the 'Daily Price Change Percentage' data field.
- **Code**:

```python
import matplotlib.pyplot as plt

# Get the data from Power BI
data = dataset['Daily Price Change Percentage']

# Create a histogram for price change distribution
plt.figure(figsize=(10, 6))
plt.hist(data, bins=30, color='purple', alpha=0.7)
plt.xlabel('Price Change Percentage')
plt.ylabel('Frequency')
plt.title('Price Change Distribution')
plt.grid(True)

# Show the plot
plt.show()

```