# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from [Luke Barousse's Python Course](https://lukebarousse.com/python) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions
Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)


# Tools I Used
For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- Python: The backbone of my analysis, allowing me to analyze the data and find critical insights. I also used the following Python libraries:

    - Pandas Library: This was used to analyze the data.
    - Matplotlib Library: I visualized the data.
    - Seaborn Library: Helped me create more advanced visuals.

- Jupyter Notebooks: The tool I used to run my Python scripts which let me easily include my notes and analysis.

- Visual Studio Code: My go-to for executing my Python scripts.

- Git & GitHub: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup
This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.
```
## Import & Clean Up Data
I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

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

## Filter US Jobs

To focus my analysis on the U.S. job market, I apply filters to the dataset, narrowing down to roles based in the United States.
```
df_US = df[df['job_country'] == 'United States']
```

# The Analysis

Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here: [2_Skill_Demand](2_Skill_Demand.ipynb).

### Visualize Data


```
fig, ax = plt.subplots(len(job_titles),1)

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short']==job_title].head(5)
    sns.set_theme(style='ticks')
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_percent', palette='dark:b_r', legend=False)

plt.show()
```

### Results

![Likelihood of skills requested](<Image\2_Likelihood of skills requested.png>)

Bar graph visualizing top 3 data roles and their top 5 skills associated with each.

### Insights:
- SQL is the most universally required skill across data roles. It appears frequently in all three jobs—Data Analyst (51%), Data Engineer (68%), and Data Scientist (51%)—showing that database querying is a foundational skill across the data industry.

- Python is especially critical for technical roles. It has the highest demand for Data Scientists (72%) and Data Engineers (65%), but much lower demand for Data Analysts (27%), indicating that programming and advanced analysis are more central in science and engineering roles.

- Business intelligence tools are more important for Data Analysts. Tools like Microsoft Excel (41%) and Tableau (28%) appear prominently only for Data Analysts, suggesting that this role focuses more on reporting, dashboards, and communicating insights to stakeholders.

## 2. How are in-demand skills trending for Data Analysts?

To find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here: [3_Skills_Trend](3_Skill_Trend.ipynb).

### Visualize Data
```
from matplotlib.ticker import PercentFormatter

df_plot = df_US_perc.iloc[:,:5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')
sns.set_theme(style='ticks')
sns.despine() #remove top and right spines

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```
### Result

![3_Skill_Trend_2](Image\3_Skills_Trend_2.png)

Bar graph visualizing the trending top skills for data analysts in the US in 2023.

### Insights:

- SQL remains the most consistently demanded skill throughout the year. It stays around 50–54% for most months, indicating that SQL is the core skill for Data Analysts and remains the most stable requirement across job postings.

- Microsoft Excel maintains strong demand but shows a noticeable decline toward the end of the year. It starts around 42–43% early in the year, drops to about 34% in November, and slightly rebounds in December, suggesting a gradual shift toward more advanced analytics tools.

- Python shows the most fluctuation and peaks in August. Demand rises to about 30–31% in August, briefly surpassing Tableau, indicating growing interest in programming and automation skills for Data Analysts, though it declines again later in the year.


# 3. How well do jobs and skills pay for Data Analysts?

To identify the highest-paying roles and skills, I only got jobs in the United States and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most.

View my notebook with detailed steps here: [4_Salary_Analysis](4_Salary_Analysis.ipynb).

### Visualize Data

```
sns.boxplot(
    data=df_US_top6,
    x='salary_year_avg',
    y='job_title_short',
    order = order
)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
### Result
![Salary Analysis_1](Image\4_Salary_Analysis_1.png)

Box plot visualizing the salary distributions for the top 6 data job titles.

### Insights

- Senior roles clearly have higher median salaries and wider ranges.
- Data Scientist roles generally command higher salaries than Data Engineer and Data Analyst roles.
- Data Analysts have the lowest median salary and narrower salary distribution.

## Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

### Visualize Data

```
fig, ax = plt.subplots(2, 1)  

sns.set_theme(style='white')

# Top 10 Highest Paid Skills for Data Analysts
# df_top_pay[::-1].plot(kind='barh', y='median', ax=ax[0], legend=False) 
sns.barplot(data=df_top_pay, x='median', y=df_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analysts
sns.barplot(data=df_skills, x='median', y=df_skills.index, hue='median', ax=ax[1], palette='light:b')
ax[1].legend().remove()
# df_skills[::-1].plot(kind='barh', y='median', ax=ax[1], legend=False)

plt.show()
```
### Result
![Salary_Analysis_2](Image\4_Salary_Analysis_2.png)

Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in the US.

### Insights
- dplyr, Bitbucket, and GitLab are the highest-paid skills, with median salaries close to $190K–$200K, indicating that analysts with expertise in advanced data manipulation (dplyr) and version control/collaboration tools (Bitbucket, GitLab) can command higher salaries.

- Most in-demand skills differ from the highest-paid ones. Tools such as Python, Tableau, SQL, and Excel appear most frequently in job requirements but have lower median salaries (around $80K–$100K), as they are standard skills expected from most data analysts.

- Specialized or complementary technical skills increase earning potential. Analysts who combine core analytics tools with advanced technologies or development workflows tend to earn higher salaries than those relying only on common analytics tools.

# 4. What are the most optimal skills to learn for Data Analysts?

To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.

View my notebook with detailed steps here: [5_Optimal_Skills](5_Optimal_Skills.ipynb).

### Visualize Data

```
from adjustText import adjust_text

plt.scatter(df_skills_high_demand['skill_percent'], df_skills_high_demand['median_salary'])
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Salary ($USD)')

ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))  # Example formatting y-axis

plt.show()
```
### Result
![5_Optimal skills](Image\5_Optimal_Skills_1.png)

A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US.

### Insights:
- Python stands out as the most optimal skill, combining a high median salary (≈ $98K) with strong demand (~35% of jobs), making it one of the most valuable skills for data analysts.

- SQL has the highest job demand (~58%) but a slightly lower median salary (≈ $91K), indicating it is a core foundational skill widely required across data analyst roles.

- Specialized tools like Oracle and SQL Server offer relatively high salaries despite lower demand, suggesting that database expertise can provide strong salary benefits even if fewer jobs require it.

### Visualizing Different Techonologies

Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})

### Visualize Data

```
from adjustText import adjust_text

sns.scatterplot(data=df_skills_tech_high_demand, x='skill_percent', y='median_salary',hue='technology')

plt.show()
```
### Result

![Optimal_Skills_2](Image\5_Optimal_Skills_2.png)

A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US with color labels for technology.

### Insights

- Programming skills provide strong value: Skills like Python and R offer relatively high salaries (around $92K–$98K) with solid demand, indicating that programming capabilities significantly improve a data analyst’s market value.

- SQL remains the most widely required skill: With the highest job share (~58%), SQL is the most demanded skill, though its median salary (~$91K) is slightly lower than some programming or cloud-related skills.

- Basic analyst tools are common but lower paying: Tools such as Excel, PowerPoint, and Word appear in many roles but are associated with lower median salaries, suggesting they are baseline productivity skills rather than specialized, high-value expertise.

# What I Learned
Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

- Advanced Python Usage: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
- Data Cleaning Importance: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- Strategic Skill Analysis: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.

### Insights
This project provided several general insights into the data job market for analysts:

- **Skill Demand and Salary Correlation**: There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.
- **Market Trends**: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
- **Economic Value of Skills**: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.

# Challenges I Faced

This project was not without its challenges, but it provided good learning opportunities:

- Data Inconsistencies: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
- Complex Data Visualization: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
- Balancing Breadth and Depth: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.

# Conclusion
This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.