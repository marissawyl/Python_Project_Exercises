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
