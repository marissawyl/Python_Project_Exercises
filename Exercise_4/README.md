# The Analysis

## 1. How did price trends change from 2013 to 2017, and what patterns can be observed when focusing on July 2017’s market movements?

To explore this, I visualized historical price data from 2013 to 2017. The upper chart shows quarterly average closing prices, capturing the overall growth pattern, while the candlestick chart below zooms into July 2017 to highlight daily fluctuations. I chose to focus on this month because it follows a sharp rise that began earlier in 2017, making it an interesting point to observe how the market behaved after that strong upward shift.

As background context, 2017 was a significant year for Bitcoin’s visibility and global market sentiment. The year saw a broad economic recovery, growing public attention on cryptocurrencies, and increased speculative interest as prices surged throughout the year. While these factors may have influenced the market, this analysis remains descriptive and focuses solely on the observable price movements shown in the data.

View my notebook with detailed steps here: [1_Closing_Price_Movements_Analysis.ipynb](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise_4/1_Closing_Price_Movements_Analysis.ipynb)

### Visualize Data

```python
# Create a line chart showing quarterly average closing prices
fig_line = px.line(
    data_frame=quarterly_resample,
    x=quarterly_resample.index,
    y='Close'
)

# Create a candlestick chart for daily price movements in July 2017
fig_candlestick = go.Candlestick(
        x=df_july.index,
        open=df_july['Open'],
        high=df_july['High'],
        low=df_july['Low'],
        close=df_july['Close']
)

# Create a subplot layout with 2 rows and 1 column:
# - Row 1: quarterly trend line
# - Row 2: detailed July candlestick chart
fig = make_subplots(
    rows=2, cols=1,
    shared_xaxes=False,
    subplot_titles=('Quarterly Average Closing Price (2013–2017)', 'Daily Price Movements – July 2017')
)

# Add the line chart traces into the first subplot
for trace in fig_line.data:
    fig.add_trace(trace, row=1, col=1)

# Highlight the period Apr–Jul 2017 with a red shaded rectangle
fig.add_shape(
    type="rect",
    x0="2017-03-31", x1="2017-07-31",
    y0=0,
    y1=quarterly_resample['Close'].max(),
    xref="x1", yref="y1",    # use x and y axis of line chart (row=1, col=1)
    fillcolor="red",
    opacity=0.2,
    layer="below",
    line_width=0
)

# Add the candlestick chart to the second subplot
fig.add_trace(fig_candlestick, row=2, col=1)

# Adjust height, remove legend, disable candlestick range slider
fig.update_layout(
    height=800,
    showlegend=False,
    title='Price Trends Overview: 2013–2017 with Detailed July 2017 Candlestick',
    xaxis2_rangeslider_visible=False, # Remove the slider of candlestick plot
    template='plotly_white'
)

fig.show()
```

### Results

![Closing_Price_Movements_Analysis](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise_4/image/Closing_Price_Movements_Analysis.png)

### Insights

- From 2013 to 2016, prices moved gradually with moderate shifts, but starting in early 2017, the growth line steepened sharply, hinting at rising confidence or new market catalysts.
- Within July 2017, daily price movements were highly volatile, with sharp dips followed by strong recoveries — indicating active trading sentiment and possible speculation during the rapid growth phase.
- The recovery toward the end of July aligned with the broader upward trajectory, suggesting continued positive sentiment throughout that period.

## 2. What can Bitcoin’s daily returns from 2013 to 2017 tell us about how its volatility evolved during its early growth years?

To dig into this, I analyzed daily return data from 2013 to 2017 to track how price fluctuations changed through time. The histogram visualizes the overall distribution of daily returns, showing how often small versus large price movements occurred. Meanwhile, the yearly boxplots compare volatility patterns across different periods, helping pinpoint when the market experienced sharper or steadier movements. The combined analysis offers a more comprehensive view of Bitcoin’s volatility dynamics throughout its early expansion period.

View my notebook with detailed steps here: [2_Volatility_Analysis.ipynb](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise_4/2_Volatility_Analysis.ipynb)

### Visualize Data

```python
# Create histogram of daily returns to visualize overall distribution
fig_histo = go.Histogram(
    x=df['Close_price_pct'],
    nbinsx=100, # Number of bin
    name='Daily Return Distribution',
    marker=dict(
        line=dict(
            color='black',
            width=1
        )
    )
)

# Create boxplot of daily returns per year to compare volatility across time
fig_boxplot = go.Box(
    x=df.index.strftime('%Y'),
    y=df['Close_price_pct'],
    boxmean='sd'    # add standard deviation line
)

# Create a subplot layout
fig = make_subplots(
    rows=2, cols=1,
    shared_xaxes=False,
    subplot_titles=('Distribution of Daily Returns from 2013 to 2017', 'Yearly Volatility Comparison of Daily Returns')
)

# Create a subplot layout
fig.add_trace(fig_histo, row=1, col=1)
fig.add_trace(fig_boxplot, row=2, col=1)

fig.update_layout(
    height=800,
    showlegend=False,
    title='Bitcoin Price Dynamics: Long-Term Trends and Short-Term Volatility (2013–2017)',
    template='plotly_white'
)

# Add axis labels to histogram
fig.update_xaxes(title_text='Daily Return (%)', row=1, col=1)
fig.update_yaxes(title_text='Frequency', row=1, col=1)

# Add axis labels to boxplot
fig.update_xaxes(title_text='Year', row=2, col=1)
fig.update_yaxes(
    title_text='Daily Return (%)',
    range=[-20, 20],    # set the y-axis values
    row=2, col=1
)

fig.show()
```

### Results

![Volatility_Analysis](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise_4/image/Volatility_Analysis.png)

### Insights

- The histogram shows a clear concentration of daily returns around zero, but with noticeable fat tails on both sides — meaning that while most price changes were modest, extreme movements were not uncommon.
- Yearly comparisons reveal that volatility remained present throughout the 2013–2017 period, though 2013 and 2017 stand out with wider ranges and more outliers, suggesting periods of heightened speculative activity.
- Despite large swings, the median daily returns across years stayed close to zero, indicating that Bitcoin’s dramatic growth phases were driven more by clusters of large positive days than by a steady upward drift.

## 3. Does higher Bitcoin trading volume tend to move hand in hand with stronger daily returns?

To explore this, I plotted daily close-to-close returns against the logarithmic scale of Bitcoin’s daily trading volume. By combining both Pearson and Spearman correlation values, I aimed to capture not just linear but also rank-based relationships between the two metrics. This visualization helps illustrate whether larger trading activity corresponds to stronger price movements or if volume and return patterns follow separate paths.

View my notebook with detailed steps here: [3_Return_Volume_Correlation_Analysis.ipynb](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise_4/3_Return_Volume_Correlation_Analysis.ipynb)

### Calculate the Pearson and Spearman values

```python
# Calculate correlation between trading volume and daily returns
pearson_val = (
    df_2014_2017[['Volume', 'Close_return']]
    .corr(method='pearson')
    .loc['Volume', 'Close_return']
)

spearman_val = (
    df_2014_2017[['Volume', 'Close_return']]
    .corr(method='spearman')
    .loc['Volume', 'Close_return']
)
```

### Visualize Data

```python
# Create scatter plot: Volume (log scale) vs Daily Returns
fig_scatter = go.Scatter(   
    x=np.log10(df_2014_2017['Volume']), # log scale reduces skew in Volume data
    y=df_2014_2017['Close_return'],
    mode='markers',
    marker=dict(size=6, color='blue', opacity=0.6)
)

# Build figure
fig = go.Figure(fig_scatter)

# Customize layout
fig.update_layout(
    height=600,
    width=1000,
    title=f'Relationship Between Daily Trading Volume and Bitcoin Daily Returns<br><sub>Pearson={pearson_val:.3f}, Spearman={spearman_val:.3f}</sub>',
    xaxis_title='Logarithmic Scale of Daily Trading Volume (BTC)',
    yaxis_title='Daily Close-to-Close Returns (%)',
    template='plotly_white'
)

fig.show()
```

### Results

![Return_Volume_Correlation_Analysis](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise_4/image/Return_Volume_Correlation_Analysis.png)

### Insights

- The scatter plot shows a wide spread of points without a clear upward or downward trend, reflecting that daily trading volume and returns don’t have a strong direct relationship.
- Both correlation coefficients (Pearson = 0.060, Spearman = 0.138) confirm this weak association, which suggesting that Bitcoin’s price swings weren’t primarily driven by short-term changes in trading volume.
- The slight positive skew hints that on some days, higher volumes coincide with larger returns, but this pattern isn’t consistent enough to indicate a stable correlation.
