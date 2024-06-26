# Overview
This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

# The Questions
Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn?

# Tools I Used
For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
1. Pandas Library: This was used to analyze the data.
2. Matplotlib Library: I visualized the data.
3. Seaborn Library: Helped me create more advanced visuals.
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code:** My go-to for executing my Python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup
## Import & Clean Up Data
I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.\
View my notebook with detailed steps here : [1_Import&Cleaning.ipynb](1_Import&Cleaning.ipynb)

## Filter US Jobs
To focus my analysis on the U.S. job market, I apply filters to the dataset, narrowing down to roles based in the United States.
```python
df_US = df[df['job_country'] == 'United States']
```
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

![Visualization of Top Skills for Data Roles](Result/Top_3_Skills.png)
*Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each*

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
![Trending Top Skills for Data Analysis in the US](Result/Skills_Trend.png)
*Bar graph visualizing the trending top skills for data analysts in the US in 2023*

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
![Salary Distributions of Data Jobs in the US](Result/Salary_Distributions.png)
*Box plot Visualizing the salary distributions for the top 6 data job titles*

### Insights
- Senior Data Scientists and Senior Data Engineers have the highest median salaries, with a range from 100k to 200k.
- Data Scientists and Data Engineers have similar median salaries, both around $150k, but have a wider range than Senior Data Scientists and Engineers.
- Data Analysts and Senior Data Analysts have lower median salaries compared to the other job titles, but still in the six-figure range.
- Across all job titles, there are a number of outliers who earn significantly more than the typical range for each position. This could be due to factors such as experience, location, and company size.

## 4. What are the most optimal skills to learn for Data Analysts?

View my notebook with detailed steps here : [6_Optimal_Skill.ipynb](6_Optimal_Skill.ipynb)

### Visualize Data 
```python
plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Salary ($USD)')  # Assuming this is the label you want for y-axis
plt.title('Most Optimal Skills for Data Analysts in the US')

# Get current axes, set limits, and format axes
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))  # Example formatting y-axis

# Add labels to points and collect them in a list
texts = []
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], " " + txt))

# Adjust text to avoid overlap and add arrows
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

plt.show()
```

### Results 
![Visualize optimal skills for Data Analyst](Result/Optimal_Skills.png)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US*

### Insights
- Python and Tableau have the highest median salaries, but are used in a smaller proportion of Data Analyst jobs.
- SQL is used in a higher proportion of jobs, but has a lower median salary.
- Excel has the lowest median salary, but is used in a relatively high proportion of Data Analyst jobs.
- Oracle has a high median salary, but is used in a relatively small proportion of jobs.
- R has a high median salary, and is used in a relatively small proportion of jobs.
- Word and PowerPoint have the lowest median salaries and are used in a smaller proportion of Data Analyst jobs.
- Go, SAS, and Power BI all have relatively high median salaries, with Go having the highest of the three.

# Recommendations for Data Professionals: 
* **Skill Development:** Data Scientists should deepen Python skills with a focus on machine learning and data visualization. Data Engineers should prioritize Python and SQL, including database design and ETL processes. Data Analysts should enhance SQL proficiency for complex queries and database management.

* **Visualization Tools:** Master Tableau for interactive dashboards and data storytelling for both Data Analysts and Scientists. Learn Power BI to diversify skills and leverage its growing demand.

* **Excel:** Focus on advanced Excel functionalities like pivot tables, VLOOKUP, and data analysis toolpak.

* **High-Demand Skills with High Salaries:** Develop expertise in Python and Tableau to boost earning potential. Gain proficiency in Oracle and R for lucrative opportunities. Learn the emerging technology Go for high salaries in backend development.

* **Career Path:** Aim for senior roles like Senior Data Scientist and Senior Data Engineer for the highest salaries. Diversify skill sets with high-paying technologies like Oracle, R, and Go. Understand factors contributing to outlier salaries, such as specialized skills and strategic locations.

* **Professional Development:** Stay updated with industry trends through continuous learning, online courses, and certifications. Network with peers and industry experts to discover job opportunities and best practices. Obtain certifications in SQL, Python, Tableau, Power BI, and Go to validate and enhance skills.