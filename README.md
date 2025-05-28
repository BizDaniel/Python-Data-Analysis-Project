# # Overview

This is an analysis of the data job market, focusing on data analyst role. This project was created because I wanted to learn something about Data Analysis and Python working on data that has to do with the world of data analysis work. This project helps to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

# The Questions

Below are the questions I want to answer in this project:
 
1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills trending for Data Analysts?
4. What are the optimal skills for Data analysts to learn? 

# Tools I Used

I used the power of several key tools:


- **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
    - **Pandas Library:** This was used to analyze the data. 
    - **Matplotlib Library:** I visualized the data.
    - **Seaborn Library:** Helped me create more advanced visuals. 
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code:** My go-to for executing my Python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup

This section outlines the steps taken to preapare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

# Filter US Jobs

To focus my analysis on the U.S. job market, I apply filters to the dataset, narrowing down to roles based in the United States

```python
df_US = df[df['job_country'] == 'United States']

```

# The Analysis

Each Jupyter Notebook for this project aimed at investigating specific aspects of the data job market. Here's how I approached each question:

## What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting. 

View my notebook with detailed steps here: [2_Skills_Demand](2_Skills_Demand.ipynb).

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```

### Results

DA METTERE IMMAGINE

### Insights:

- SQL is the most requested skill for Data analysts and Data Scientists, with it in over half the job postings for both roles. For Data Engineers, Python is the most sought-after skill, appearing in 60% of job postings.
- Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).
- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%).

## 2. How are in-demand skills trending for Data Analysts?

To find how skills are trending in 2023 for Data Analysts and Data Scientists, I filtered the data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here: [3_Skills_Trend](3_Skills_trend.ipynb).

### Visualize Data

```python

from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()

```

### Results

METTERE IMMAGINE

### Insights:

- SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand. 
- Excel experienced a significant increase in demand starting around September, surpassing both Python and Tableau by the end of the year.
- Both Python and Tableau show relatively stable demand throughout the year with some fluctuations but remain essential skills for data analysts. Power BI, while less demanded compared to the others, shows a slight upward trend towards the year's end.

## 3. How well do jobs and skills pay for Data Analysts?

To identify the highest-paying roles and skills, I only got jobs in the United States and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get and idea of which jobs are paid most.

View my notebook with detailed steps here: [4_Salary_Analysis](4_Salary_Analysis.ipynb).

#### Visualize Data

```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()

```

#### Results

METTERE IMMAGINE

#### Insights

- There's a significant variation in salary ranges across different job titles. Senior Data Scientist position tend to have the highest salary potential, with up to $600K, indicating the high value placed on advanced data skills and experience in the industry.

- Senior Data Engineer and Senior Data Scientist role show a considerable number of outliers on the higher end of the salary spectrum, suggestin that exceptional skills or circumstances can lead to high pay in these roles. In contrast, Data analyst roles demonstrate more consistency in salary, with fewer outliers.

- The median salaries increase with seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries, reflecting greater variance in compensation ad responsibilities increase.

### Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed my analysis and focused only on data analyst role. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

#### Visualize Data

```python

fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analystsr')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()

```

#### Results 

Here's the breakdown of the highest-paid & most in-demand skills for data analysts in the US:

METTERE IMMAGINE

#### Insights 

- The top graph shows specialized technical skills like `dplyr`, `Bitbucket`, and `Gitlab` are associated with higher salaries, some reaching up to $200K, suggesting that advanced technical proficiency can increase earning potential.

- The bottom graph highlights that foundational skills like `Excel`, `PowerPoint`, and `SQL` are the most in-demand, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for employability in the data analysis roles.

- There's a clear distinction between the skills that are the highest paid and those that are most in-demand. Data Analysts aiming to maximize their career potential should consider developing a diverse skill set that includes both high-paying specialized skills and widely demanded foundational skills.

## 4. What are the most optimal skills to learn for Data Analysts?

To identify the most optimal skills to learn (the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.

View my notebook with detailed steps here: [5_Optimal_Skills](5_Optimal_Skills.ipynb).

#### Visualize Data

```python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()

```

#### Results

METTERE IMMAGINE

#### Insights:

- The skill `Oracle` appears to have the highest median salary of nearly $97K, despite being less common in job postings. This suggests a high value placed on spacialized database skills within data analyst profession.

- More commonly required skills are `Excel` and `SQL` have a large presence in job listings but lower median salaries compared to specialized skills like `Python` and `Tableau`, which not only have higher slaries but are also moderately prevalent in job listings.

- Skills such as `Python`, `Tableau`, and `SQL Server` tend to be linked with higher salaries and are also quite common in job listings. This suggests that knowing these tools can help you find good opportunities in data analytics.

### Visualizing Different Technologies

Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming, Python})

#### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

# Create a scatter plot
scatter = sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology',  # Color by technology
    palette='bright',  # Use a bright palette for distinct colors
    legend='full'  # Ensure the legend is shown
)
plt.show()

```

#### Results

METTERE IMMAGINEE

#### Insights:

- The scatter plot shows that most of the  `programming` skills (colored blue) are grouped around higher salary levels compared to other types of skills. This suggests that knowing how to program can lead to better salaries in data analytics.

- The database skills (orange ones), such as `Oracle` and `SQL Server`, are associated with some of the highest salaries among the data analyst tools. That means that there's an high demand and valuation for professionts in data management and manipulation.

- Analyst tools (colored green), including `Tableau` and `Power BI`, are very common in job postings and offer competitive salaries, showing that visualization and data analysis software are fundamental for data roles. This category not only has good salaries but is also versatile across different types of data tasks.

# What I Learned

I came across the project to improve my knowledge about data analysis. I understood the data analyst job market and I get better my technical skills in Python, especially in data manipulation and visualization. 

# Insights 

This project provided several general insights into the data job market for analysts:

- **Skill Demand and Salary Correlation**: There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.
- **Market Trends**: The demand fro skills is always changing. Staying up to date is important for growing your career in data analytics
- **Economic Value of Skills**: Understanding which skills are both in-demand and well-compensated can lead a data analyst to improve some certain skills.

# Challenges I faced

There was some basic problems that hindered the developing of this project:

- **Data Inconsistencies**: Dealing with missing or incosistent data needed careful attention and good cleaning methods to make sure the analysis was accurate.
- **Complex Data Visualization**: Making clear and useful charts for complex data was difficult but very important for sharing insights in an easy-to-understand way.
- **Balancing Breadth and Depth**: It was a challenge to decide how muhc detail to go into while still keeping a full view of the whole dataset. I had to find the right balance to cover everything without getting too caught up in small details.

# Conclusion

this look into the data analyst job market has been very helpful. It shows the key skills and trends that shape this changing field. The insights I found help me understand it better and give useful tips for anyone who wants to grow their career in data analytics. As the market keeps changing, it's important to keep analyzing it to stay ahead. This project is a good starting point for future work and highlights how important it is to keep learning and adapting in the data world.
