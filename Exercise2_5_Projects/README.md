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

![1_Roles_by_Country.ipynb] (https://github.com/marissawyl/Python_Project_Exercises/blob/main/Exercise2_5_Projects/images/Roles_by_Country.png)

### Insights

- Data Engineer roles dominate in India, Canada, and the Netherlands, suggesting a high demand for professionals with engineering and data pipeline skills in these regions. Companies are likely prioritizing building and maintaining data infrastructure over analysis.
- Data Analyst positions are relatively lower in India and Canada, but they are more prominent in the United States and France. This indicates that while analysis skills are important, the need for data engineering expertise is currently driving the market in several countries.
- Data Scientist roles maintain a moderate, steady presence across all countries, showing that while specialized analytics and machine learning skills are valued, the foundational work of managing and engineering data remains a top priority for employers.
