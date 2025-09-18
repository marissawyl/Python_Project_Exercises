# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

To figure this out, I first identified the top three roles based on popularity, then extracted the five most in-demand skills for each. The results highlight the leading job titles along with their key skills, helping me understand which areas to focus on depending on the role I want to pursue.

View my notebook with detailed steps here: [1_Skills_Count.ipynb](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise1_4_Projects/1_Skills_Count.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_pct[df_skills_pct['job_title_short'] == job_title].head()
    sns.barplot(data=df_plot, x='skill_pct', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r', legend=False)
    ax[i].set_xlabel('')
    ax[i].set_ylabel('')
    ax[i].set_xlim(0, 78)
    ax[i].set_title(job_title)

    for n, v in enumerate(df_plot['skill_pct']):
        ax[i].text(v + 1, n, f'{v:.1f}%', va='center')
    
    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize=15)
fig.tight_layout()
plt.show()
```

### Results

![Skill_Count](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise1_4_Projects/images/Skills_Count.png)

### Insights

- SQL is the most requested skill for Data Analysts (50.8%) and Data Engineers (68.3%), while Python dominates for Data Scientists (72.0%) and is also highly demanded for Data Engineers (64.9%). This shows that mastering both is essential in any data career path.
- Data Analysts rely more on Excel (40.6%) and Tableau (28.5%) for reporting and visualization, while Data Engineers are expected to know cloud tools like AWS (42.8%) and Azure (32.3%). Data Scientists, on the other hand, complement Python and SQL with R (44.2%) for statistical analysis.
- Data Analysts are typically expected to be proficient in business-oriented tools such as Excel and Tableau, which support reporting and data visualization. Data Engineers, in contrast, are more often required to work with cloud platforms and big data technologies, including AWS, Azure, and Spark, reflecting the technical nature of building and maintaining data infrastructure. Data Scientists, meanwhile, are frequently expected to bring strong statistical expertise through programming languages like R as well as specialized statistical software such as SAS.


## 2. How are in-demand skills trending for Data Analysts?

To explore the skill trends for Data Analysts in 2023, I focused specifically on job postings for that role and organized the required skills based on the month they were listed. This approach highlighted the 5 most in-demand skills each month, providing a clear view of how their popularity shifted over the year.

View my notebook with detailed steps here: [2_Skills_Trend.ipynb](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise1_4_Projects/2_Skills_Trend.ipynb)

### Visualize Data

```python
sns.set_theme(style='ticks')

sns.lineplot(data=df_plot, dashes=False, legend=False, palette='hls')
sns.despine()
plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')

last_values = df_plot.iloc[-1, :]

offsets = {}

# Manually set the labels for 'tableau' and 'python' since they couldn’t be adjusted with adjustText
for i, val in enumerate(last_values):
    floored = math.floor(val)
    if floored not in offsets:
        offsets[floored] = 0
    else:
        offsets[floored] += 2

    y = val - offsets[floored]
    plt.text(11.2, y, df_plot.columns[i], va='center')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```

### Results

![Skill_Trend](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise1_4_Projects/images/Skills_Trend.png)

### Insights

- SQL consistently remained the most in-demand skill for Data Analysts throughout 2023, though it showed a slight downward trend over the year. This highlights how SQL continues to be the core technical requirement for analysts despite small fluctuations.
- Excel maintained a stable position as the second most requested skill, showing its continued relevance in day-to-day analysis and reporting tasks. However, it experienced a noticeable dip in the later months before recovering slightly in December.
- Python and Tableau followed a similar pattern with moderate demand, occasionally overlapping each other, while SAS consistently ranked the lowest. This suggests that while programming and visualization skills are valued, traditional tools like SAS are becoming less prioritized compared to modern alternatives.


## 3. How well do jobs and skills pay for Data Analysts?

To determine which roles and skills offer the highest salaries, I focused on job postings from the US and analyzed their median pay. Before diving into the details, I first examined the salary distributions for common data positions such as Data Scientist, Data Engineer, and Data Analyst to understand which roles typically command the highest compensation.

View my notebook with detailed steps here: [3_Salary_Analysis.ipynb](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise1_4_Projects/3_Salary_Analysis.ipynb)

### Visualize Data

```python
sns.boxplot(data=df_data_roles, x='salary_year_avg', y='job_title_short', order=median_salary.index)
sns.despine()
plt.xlim(0,600000)
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.title('Salary Distributions of Data Jobs in the US', fontsize=15)

plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))

plt.show()
```

### Results

![Salary_Analysis](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise1_4_Projects/images/Salary_Analysis_0.png)

### Insights

- Senior roles clearly command higher salaries across the board, with Senior Data Scientists and Senior Data Engineers showing the largest median salaries. This highlights how experience and advanced expertise significantly increase earning potential in the data field.
- Among non-senior positions, Data Scientists generally earn more than Data Engineers and Data Analysts, indicating that analytical and modeling skills tend to be valued higher than reporting or infrastructure-focused roles.
- An interesting observation is that Senior Data Analysts still earn less on average than non-senior Data Scientists and Data Engineers. This suggests that technical and modeling expertise in engineering and science roles tend to be rewarded more highly than seniority in purely analytical positions.



Next, I refined the analysis to focus exclusively on data analyst positions. I examined both the skills that command the highest salaries and those that are most frequently requested. To illustrate these findings, I presented the results using two bar charts.
