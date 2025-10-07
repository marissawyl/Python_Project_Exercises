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
