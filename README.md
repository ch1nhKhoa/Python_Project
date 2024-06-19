# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

View my notebook with detailed steps here : [3_Skill_Demand.ipynb](3_Skill_Demand.ipynb)

### Visualize Data 
```python

sns.set_theme(style='ticks')

fig, ax = plt.subplots(len(job_titles), 1, figsize=(10, len(job_titles)*2))

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].invert_yaxis()
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].set_xlim(0, 78)  # Adjust xlim if necessary to fit your data range

    # remove the x-axis tick labels for better readability
    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

    # label the percentage on the bars
    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')

fig.suptitle('Counts of Skills Requested in US Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5)  # Adjust padding between subplots
plt.show()
```

### Results

![Visualization of Top Skills for Data Roles](Result\Top_3_Skills.png)

### Insights 
- Python and SQL: These two skills dominate the landscape across all roles. Python is particularly emphasized for Data Scientists (72%) and Data Engineers (65%), while SQL is critical for Data Analysts (51%) and Data Engineers (68%).
- Visualization Tools: Tableau appears for both Data Analysts (28%) and Data Scientists (24%), indicating the importance of data visualization across these roles.

## 2. How are in-demand skills trending for Data Analysts?

View my notebook with detailed steps here : [4_Skill_Trend.ipynb](4_Skill_Trend.ipynb)

### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')
sns.set_theme(style='ticks')
sns.despine() # remove top and right spines

plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

# Annotate the plot with the top 5 skills using plt.text()
for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i], color='black')

plt.show()
```

### Results
![Trending Top Skills for Data Analysis in the US](Result\Skills_Trend.png)

### Insights
- SQL is the most in-demand skill, though demand is decreasing slightly over time.
- Excel demand is increasing, especially after September, and surpassed Python and Tableau by December.
- Python and Tableau demand are relatively stable throughout the year, with some fluctuations.
- Power BI demand is less than other skills but shows a slight upward trend by December.

## 3. How well do jobs and skills pay for Data Analyst ?

View my notebook with detailed steps here : [5_Salary_Analysis.ipynb](5_Salary_Analysis.ipynb)

### Visualize Data
```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')
sns.despine()

# this is all the same
plt.title('Salary Distributions of Data Jobs in the US')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 600000) 
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)

plt.show()

```

### Results
![Salary Distributions of Data Jobs in the US](Result\Salary_Distributions.png)
*Box plot Visualizing the salary distributions for the top 6 data job titles*

### Insights
- Senior Data Scientists and Senior Data Engineers have the highest median salaries, with a range from 100k to 200k.
- Data Scientists and Data Engineers have similar median salaries, both around $150k, but have a wider range than Senior Data Scientists and Engineers.
- Data Analysts and Senior Data Analysts have lower median salaries compared to the other job titles, but still in the six-figure range.
- Across all job titles, there are a number of outliers who earn significantly more than the typical range for each position. This could be due to factors such as experience, location, and company size.