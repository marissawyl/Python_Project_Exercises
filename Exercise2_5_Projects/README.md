# Introduction

This project is part of my journey to deepen my Python and data analysis skills. I explored a dataset of job postings in 2024 provided by Luke Barousse to uncover trends and insights in the Indian job market. Through this project, I practiced cleaning, analyzing, and visualizing real-world data while learning how to translate findings into meaningful insights.

# Background

The goal of this analysis is to explore the demand for Data Analyst roles in India in 2024. While data engineering positions show the highest demand, I chose to focus on Data Analyst roles for several reasons:  
1. **Learning focus**: This project is primarily about honing data analysis skills, including data cleaning, exploration, and visualization.  
2. **Actionable insights**: Understanding trends in data analyst positions can help beginners or career switchers navigate the job market.  
3. **Manageable scope**: Analyzing Data Engineer roles could require deeper technical context and a broader skill set, which is beyond the scope of this learning project.  

By focusing on Data Analyst roles, I could concentrate on developing strong analytical and Python skills while still providing meaningful insights about the job market.

# Tools Used

I used Python along with essential libraries such as Pandas for data manipulation, and Matplotlib and Seaborn for data visualization. The project was developed in Visual Studio Code, which provided a robust environment for coding, testing, and visualizing results efficiently. This combination of tools allowed me to explore the dataset and generate professional, clear visualizations.

# The Analysis

## 1. Which data-related job roles are most in demand across the top countries in 2024, and how does their popularity vary by country?

To address this question, I first identified the top 3 job roles based on their prevalence in job postings, then compared their distribution across leading countries. By visualizing the likelihood of each role being posted in different regions, I could observe regional trends and high-demand positions, helping inform strategic career or business decisions in the global data industry.

View my notebook with detailed steps here: [1_Roles_by_Country.ipynb](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise2_5_Projects/1_Roles_by_Country.ipynb)

### Visualize Data

```python
sns.set_theme(style='ticks')

g = sns.catplot(
    data=df_merged,
    kind='bar',
    x='job_country',
    y='title_pct',
    hue='job_title_short',
    palette='tab10',
    order=top_countries
)
g._legend.set_title('Job Roles')
g._legend.set_loc('center right')

plt.xticks(rotation=45, ha='right')
plt.xlabel('')
plt.ylabel('Likelihood in Job Posting')
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))
plt.title(f'Distribution of Data Job Roles Across Top {top_n} Countries (2024)', fontsize=15)
plt.grid(True, axis='y', linestyle='--', alpha=0.7)

plt.show()
```

### Results

![Roles_by_Country](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise2_5_Projects/images/Roles_by_Country.png)

### Insights

- Data Engineer roles dominate in India, Canada, and the Netherlands, suggesting a high demand for professionals with engineering and data pipeline skills in these regions. Companies are likely prioritizing building and maintaining data infrastructure over analysis.
- Data Analyst positions are relatively lower in India and Canada, but they are more prominent in the United States and France. This indicates that while analysis skills are important, the need for data engineering expertise is currently driving the market in several countries.
- Data Scientist roles maintain a moderate, steady presence across all countries, showing that while specialized analytics and machine learning skills are valued, the foundational work of managing and engineering data remains a top priority for employers.

## 2. Which skills are driving demand and top salaries for Data Analysts in India in 2024?

To explore this, I looked at both the most in-demand and highest-paying skills for Data Analysts. I compared the skills that appear most frequently in job postings with those linked to the top salaries. This helps identify which skills can boost employability, which can maximize earning potential, and where to focus for career growth.

View my notebook with detailed steps here: [2_Salary_by_Skills.ipynb](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise2_5_Projects/2_Salary_by_Skills.ipynb)

### Visualize Data

```python
sns.set_theme(style='ticks')
fig, ax = plt.subplots(2, 1)

sns.barplot(
    data=df_grouped_median,
    x='median_salary',
    y='job_skills',
    ax=ax[0],
    hue='median_salary',
    palette='dark:b_r',
    legend=False
)
ax[0].set_xlabel('')
ax[0].set_title('Highest Paid Skills')

for idx, v in enumerate(df_grouped_median['median_salary']):
    ax[0].text(v + 2000, idx, f'${(v/1000):.1f}k', va='center', fontsize=10)

sns.barplot(
    data=df_grouped_cnt,
    x='median_salary',
    y='job_skills',
    ax=ax[1],
    hue='median_salary',
    palette='light:b',
    legend=False
)
ax[1].set_xlabel('Median Salary (USD)')
ax[1].set_xlim(0, 160000)
ax[1].set_title('Most In-Demand Skills')

for idx, v in enumerate(df_grouped_cnt['median_salary']):
    ax[1].text(v + 2000, idx, f'${(v/1000):.1f}k', va='center', fontsize=10)

for i, axis in enumerate(ax):
    axis.set_ylabel('')
    axis.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))

    # Set x-ticks only on the last subplot
    if i != len(ax) - 1:
        axis.set_xticks([])

sns.despine()
plt.suptitle('Top-Paying vs Most Popular Skills for Data Analysts in India (2024)', fontsize=15)

fig.tight_layout()
plt.show()
```

### Results

![Salary_by_Skills](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise2_5_Projects/images/Salary_by_Skills.png)

### Insights

- High-paying skills like TensorFlow, C, C++, JavaScript, and Jupyter offer significant salary potential for Data Analysts, even though they are not necessarily the most common skills in job postings. This suggests that specializing in these areas can give a competitive edge.
- The most in-demand skills, including SAP, Python, R, SQL, and Excel, are widely sought after in job postings but tend to offer lower median salaries compared to the high-paying skills. This indicates that proficiency in these tools is essential for landing a role, even if they do not maximize earnings immediately.
- There is a clear gap between high salary skills and high demand skills, highlighting a strategic choice for career development: focusing on popular skills may increase employability, while mastering high-paying, less common skills can boost earning potential for specialized roles.

## 3. Which data roles in India are most likely to provide remote work options in 2024, and how does this vary across data roles?

This analysis compares the remote job share for Data Analysts, Data Engineers, and Data Scientists. Understanding this distribution provides valuable context for both job seekers exploring flexible opportunities and organizations evaluating how their offerings align with market expectations.

View my notebook with detailed steps here: [3_Remote_Jobs.ipynb](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise2_5_Projects/3_Remote_Jobs.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(1, 3, figsize=(10,3), constrained_layout=True)

for i, role in enumerate(job_roles):
    wfh_count = df_2024_IN[df_2024_IN['job_title_short'] == role]['job_work_from_home'].value_counts().reindex([False, True], fill_value=0)
    ax[i].pie(wfh_count, startangle=90, labels=['Not Remote', 'Remote Available'], autopct='%1.1f%%')
    ax[i].set_title(role)

fig.suptitle('Share of Remote Job Postings by Role in India (2024)', fontsize=15)
plt.show()
```

### Results

![Remote_Jobs](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise2_5_Projects/images/Remote_Jobs.png)

### Insights

- Data Analysts stand out with the highest proportion of remote-friendly roles at 20.5%. This suggests that companies are more open to offering remote work for analysts compared to other data roles.
- Data Engineers show the lowest availability of remote options, with only 9.7% of postings. This could indicate that engineering work often requires closer collaboration with infrastructure teams, making remote setups less common.
- Data Scientists fall in between, with 11.7% of postings mentioning remote work. While slightly higher than engineers, the gap compared to analysts highlights that analytical roles may be more adaptable to remote environments.

## 4. What do the 2024 hiring trends reveal about the most sought-after data roles in India?

To explore this, I examined the monthly distribution of job postings across six major data roles. Looking at the job postings across 2024 makes it possible to see which roles were consistently in demand and which spiked only at certain times of the year. These patterns offer useful guidance for anyone deciding where to focus their skills and career development in the data field.

View my notebook with detailed steps here: [4_Job_Roles_by_Month.ipynb](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise2_5_Projects/4_Job_Roles_by_Month.ipynb)

### Visualize Data

```python
sns.set_theme(style='ticks')

ax = sns.lineplot(data=df_pivot, dashes=False)
sns.despine()

plt.title('Monthly Trends of Data-related Job Postings in India (2024)', fontsize=15)
plt.xlabel('2024')
plt.ylabel('Number of Job Postings')
plt.legend(
    title='Job Roles',
    loc='upper right',
    bbox_to_anchor=(1.4, 1)
)

# Find the max value for each job role
for job_role in df_pivot.columns:
    y_max1 = df_pivot[job_role].max()
    x_max1 = df_pivot[job_role].idxmax()

    ax.scatter(x_max1, y_max1, color='red', zorder=5)
    ax.text(x_max1, y_max1 + 50, y_max1, ha='center', color='red', fontsize=9, fontweight='bold')

plt.show()
```

### Results

![Job_Roles_by_Month](https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise2_5_Projects/images/Job_Roles_by_Month.png)

### Insights

- Data Engineer consistently led the market with the highest number of postings, peaking at 1,771 in August. This trend confirms that engineering expertise in building and maintaining scalable data systems remains the backbone of the data job market in India.
- Senior roles such as Senior Data Scientist and Senior Data Engineer showed steady demand throughout the year, though at a lower volume compared to mid-level positions. Their stability indicates that while fewer openings exist, these roles remain critical for organizations investing in leadership and advanced expertise.
- Data Scientist demand surged notably in August, reaching 1,175 postings, showing that companies heavily prioritized advanced analytics talent mid-year. In contrast, Data Analyst roles peaked earlier in March (641 postings) and then declined, suggesting that entry-level and junior positions are more sensitive to market shifts.
