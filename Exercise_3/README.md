# Introduction

This project is part of my learning journey to improve my Python skills for data analysis. It’s uses the Uber dataset from New York City covering the years 2014–2015, originally shared by Shan Singh on Udemy. Through this project, I aim to practice data wrangling, exploratory analysis, and visualization to better understand real-world datasets.

# Background

Ride-hailing platforms like Uber generate a large amount of data every day. Analyzing such data can provide valuable information about passenger demand, peak hours, and how driver supply responds to that demand. In this project, I focused on understanding when and where demand is highest, identifying rush hour patterns across weekdays and months, and comparing the relationship between active vehicles and completed trips. Beyond being a technical practice, this project also demonstrates how Python can be applied to real-life scenarios.

# Tools Used

The analysis was carried out in Visual Studio Code using Jupyter Notebook. Python was the main language for the project, supported by several libraries for data analysis and visualization. Pandas was used for data cleaning and transformation, NumPy for numerical operations, and Matplotlib together with Seaborn for creating visualizations.

# The Analysis

## 1. How does ride demand shift across months, and do weekdays show any clear differences in pickup volume?

To answer this, I compared ride demand across months and broke it down by day of the week. By visualizing monthly trends alongside weekday patterns, I could see both overall growth and the weekly distribution of activity. This helped me identify not only when demand peaked, but also which days consistently drove higher volumes.

View my notebook with detailed steps here: [1_Uber_Ride_Demand_Trends.ipynb](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise_3/1_Uber_Ride_Demand_Trends.ipynb)

### Visualize Data

```python
# Apply seaborn theme for cleaner visualization
sns.set_theme(style='ticks')

# Create bar plot: x = month, y = count, hue = weekday
ax = sns.catplot(
    data=df_pickup_grouped,
    kind='bar',
    x='Pickup_month',
    y='Pickup_cnt',
    hue='Pickup_day',
    palette='colorblind',   # colorblind-friendly palette
    edgecolor='black',
    height=6,
    aspect=1.5              # make plot wider
)

# Update legend position and title
ax.legend.set_title('Day of Week')
ax.legend.set_loc('upper right')

# Add plot title and axis labels
plt.title('Uber Ride Demand Trends by Month and Weekday', fontsize=15, weight='bold')
plt.xlabel('Pickup Month')
plt.ylabel('Number of Pickups')

# Format y-axis with thousands separator
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'{y:,.0f}'))

plt.show()
```

### Results

![Uber_Ride_Demand_Trends](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise_3/images/Uber_Ride_Demand_Trends.png)

### Insights

- Ride demand clearly increases over time, with noticeable growth from January through May, before stabilizing slightly in June. This suggests a steady rise in usage during the first half of the year.
- Fridays and Saturdays consistently record the highest number of pickups each month, highlighting weekends as the busiest period for ride demand.
- Mondays show the lowest demand compared to other weekdays, reinforcing the idea that commuting patterns alone do not drive as much activity as social or leisure-related travel later in the week.

## 2. What are the peak demand hours for Uber rides in NYC, and how do they vary across weekdays and weekends?

To explore this, I grouped Uber pickup data from New York City (Jan–Jun 2015) by both the hour of the day and the day of the week. By plotting these trends, it became possible to observe how demand shifts throughout the day and compare weekdays with weekends. This visualization highlights not only the daily peaks but also the differences in behavior between weekdays and weekends, providing a better understanding of urban ride-hailing demand.

View my notebook with detailed steps here: [2_Rush_Hour_by_Weekdays.ipynb](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise_3/2_Rush_Hour_by_Weekdays.ipynb)

### Visualize Data

```python
# Set Seaborn theme for cleaner style
sns.set_theme(style='ticks')
plt.figure(figsize=(10, 5))

# Create a point plot → each line shows number of pickups per hour for each weekday
g = sns.pointplot(
    data=df_grouped,
    x='Pickup_hour',
    y='size',
    hue='Pickup_day',
    palette='tab10',
    legend=False        # turn off legend, labels will be added manually
)
sns.despine()   # remove top and right borders for a clean look

# Add plot title and axis labels
plt.suptitle('Hourly Ride Demand by Day of Week', fontsize=15, weight='bold')
plt.title('New York City Uber Pickups (Jan–Jun 2015)', fontsize=13, style='italic')
plt.xlabel('Pickup Hour (0–23)')
plt.ylabel('Number of Pickups (rides)')

# Format y-axis with thousands separator
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'{y:,.0f}'))

# === Add manual labels at the end of each line (on the right side) ===
texts = []

for i, text in enumerate(df_grouped['Pickup_day'].unique()):
    y_axis = df_grouped[df_grouped['Pickup_hour'] == 23]['size'].iloc[i]
    texts.append(plt.text(23.5, y_axis, text, va='center'))

# Adjust labels to avoid overlapping
adjust_text(texts)

# Shift labels a bit further right for clarity
for t in texts:
    t.set_x(23.8)

plt.show()
```

### Results

![Rush_Hour_by_Weekdays](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise_3/images/Rush_Hour_by_Weekdays.png)

### Insights

- The lowest demand appears consistently around 3–5 AM across all days, when the city is mostly inactive. This shows a universal "quiet period" regardless of the weekday or weekend.
- On weekdays, there is a sharp rise in pickups during morning hours (7–9 AM) that aligns with commuting times, followed by another peak in the late afternoon and evening (5–8 PM). This reflects typical work-related travel behavior.
- Weekends show a different pattern: demand continues to rise steadily throughout the day, reaching the highest levels late at night, especially on Friday and Saturday. This suggests that social and leisure activities drive late-night demand compared to weekday commuting peaks.
