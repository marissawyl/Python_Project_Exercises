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
