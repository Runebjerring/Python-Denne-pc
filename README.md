# Overview

Welcome to my analysis of the data job market, with a focus on data analyst roles. This project was created to better navigate and understand the job market, specifically by identifying the top-paying and in-demand skills to help data analysts find optimal job opportunities.

The data was sourced from https://www.lukebarousse.com/python, which provides a foundation for my analysis. It includes detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

The data has a close to global scope, but the majority of the analysis is based on data related to the United States. 

# The Questions 

Below are the questions I aim to answer in my project:

1. What are the most in-demand skills for the top 3 most
common data roles?

2. How are in-demand skills trending for Data Analysts?

3. What is the salary landscape for Data Analysts and the skills they possess?

4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)

# Tools I Used

For my deep dive into the data analyst job market, I utilized several key tools:

- **Python**: The backbone of my analysis, allowing me to analyze the data and uncover critical insights. I also used the following Python libraries:

   - Pandas Library: This was used to analyze the data.
   - Matplotlib Library: I visualized the data.
   - Seaborn Library: Helped me create more advanced visuals.

- Jupyter Notebooks: To run my Python scripts, allowing for seamless integration of notes and analysis.

- Visual Studio Code: My go-to environment for executing Python scripts.

- Git & GitHub: Essential for version control, sharing my Python code, and maintaining collaboration and project tracking

# Data Preparation and Cleanup
This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data: 
I began by importing the necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

``` python
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
## Filter for US Jobs

To focus my analysis on the U.S. job market, I apply filters to the dataset, narrowing down to roles based in the United States.

``` python

df_US = df[df['job_country'] == 'United States']

```

# Analysis 

Each Jupyter Notebook in this project was designed to investigate specific aspects of the data job market. Here is the approach taken for each research question:

## 1. What are the most in-demand skills for the top 3 most frequent data roles?

To determine the most in-demand skills for the top 3 most frequent data roles, I first identified the roles by ranking the job titles based on their frequency. Then, I extracted and analyzed the top 5 skills for each of these top 3 roles. This analysis highlights the key skills required for the most sought-after positions, providing insights into which skills are essential for aspiring professionals targeting these roles.

View my notebook with detailed steps here:
[2_Skill_demand.ipynb](3_project\2_Skills_count.ipynb)


### Visualize Data
``` python 
fig, ax = plt.subplots(len(job_titles), 1)

sns.set_theme(style='ticks')

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] ==job_title].head(5)


    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0,78)
    ax[i].legend().set_visible(False)


    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')

    if i != len(job_titles)-1:
        ax[i].set_xticks([])

fig.suptitle('Likelihood of Skills in Job Posting', fontsize=15)
fig.tight_layout(h_pad=0.5)
plt.show() 
``` 

### Results
![Visualization of Top Skills for Data Oriented Jobs](3_project/Images%20from%20project/Likelihood_of_skill_demand_all_data_roles.png)

*Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each*

### Insights 

* SQL is the most requested skill for Data Analysts and Data Engineers, appearing in over half of the job postings for both roles. For Data Engineers, SQL is the most sought-after skill, featured in 68% of job postings.

* Data Engineers require more specialized technical skills such as AWS, Azure, and Spark, in contrast to Data Analysts and Data Scientists, who are expected to be proficient in more general data management and analysis tools like Excel and Tableau.

* Python is a versatile skill, highly demanded across all three roles, but it is most prominently required for Data Scientists (72%) and Data Engineers (65%).

## 2. How are in-demand skills trending for Data Analysts?

### Visualize Data

``` python

df_plot = df_DA_US_percent.iloc[:, :5]

sns.lineplot(data=df_plot, dashes=False, palette='tab10') 
sns.set_theme(style='ticks')
sns.despine()

plt.title('Trending top Skills for Data Analyst in the US monthly (Jan-Dec)')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))


for i in range(5):
    plt.text(12.3, df_plot.iloc[-1, i],df_plot.columns[i])

 ``` 
 ### Results

![Trending Top Skills for Data Analysts in the United States](3_project/Images%20from%20project/skill_trend_DA.png)

*Line graph visualizing the trending top skills for data analyst in the US in 2023.*


### Insights 
* SQL remains the most consistently in-demand skill throughout the year, although it shows a gradual decrease in demand over time. 

* Excel experienced a significant increase in demand starting around September, eventually surpassing both Python and Tableau by the end of the year.

* Both Python and Tableau maintain relatively stable demand throughout the year, with some fluctuations, but they remain essential skills for data analysts. Power BI, while less in-demand compared to the other tools, shows a slight upward trend towards the end of the year.


## 3. How well do job and skills pay for Data Analysts?

### Salary Analysis for Data Oriented Jobs

#### Visualize Data

``` python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
#### Results 

![Salary Distribution of Data Jobs in the US](3_project/Images%20from%20project/Salary_distribution_Data_jobs_US.png)

*Box plot visualizing the salary distributions for the top 6 data job titles* 

#### Insights

* There is significant variation in salary ranges across different job titles. Senior Data Scientist positions tend to have the highest salary potential, reaching up to $600K, indicating the high value placed on advanced data skills and extensive experience in the industry.

* Senior Data Engineer and Senior Data Scientist roles exhibit a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to substantially higher pay in these roles. In contrast, Data Analyst roles demonstrate more consistency in salary, with fewer outliers.

* The median salaries increase with the seniority and specialization of the roles. Senior positions (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also exhibit greater variance in compensation, reflecting the increased responsibilities and the broader range of skills required

## 4. Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

#Visualize Data

fig, ax = plt.subplots(2, 1)  

``` python
# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analysts')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()
```

#### Results

![The Highest Paid & Most In-Demand Skills for Data Analysts in the US](3_project/Images%20from%20project/Top_10_Highest_Paid_Skills_for_Data_Analysts.png)

*Two seperate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in the US.*

#### Insights 

* The top graph indicates that specialized technical skills such as dplyr, Bitbucket, and Gitlab are associated with higher salaries, some reaching up to $200K. This suggests that advanced technical proficiency can significantly increase earning potential.

* The bottom graph highlights that foundational skills such as Excel, PowerPoint, and SQL are the most in-demand, even though they may not offer the highest salaries. This underscores the importance of these core skills for employability in data analysis roles.

* There is a clear distinction between the skills that command the highest salaries and those that are most in-demand. Data analysts aiming to maximize their career potential should consider developing a diverse skill set that includes both high-paying specialized skills and widely demanded foundational skills.


## 5. What are the most optimal skills to learn for Data Analysts?
To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.

View my notebook with detailed steps here: [ 5_optimal_skills.ipynb](3_project\5_Optimal_skills.ipynb)


### Visualize Data
``` python 
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()
```

### Results

![Most Optimal Skills for Data Analyst in the US](3_project/Images%20from%20project/Most_Optimal_Skills_for_Data_Analyst_in_the_US.png)

*A scatter plot visualizing the most optimal skills (high paying & high demand) for Data Analysts in the US.*


### Insights:

* The skill Oracle appears to have the highest median salary of nearly $97K, despite being less common in job postings. This suggests that specialized database skills are highly valued within the data analyst profession.

* More commonly required skills, such as Excel and SQL, have a significant presence in job listings but offer lower median salaries compared to specialized skills like Python and Tableau. These specialized skills not only command higher salaries but are also moderately prevalent in job postings.

* Skills such as Python, Tableau, and SQL Server are towards the higher end of the salary spectrum while also being fairly common in job listings, indicating that proficiency in these tools can lead to attractive opportunities in data analytics.

## Visualizing Different Techonologies

A different visuilization from the project shows the different technologies as well in the graph. Color is added to labels based on the technology (e.g, {Programming: Python})

## Visualize

from matplotlib.ticker import PercentFormatter

# Create a scatter plot

``` python
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

### Results

![Most Optimal Skills for Data Analyst in the US - Technology oriented](3_project\Images%20from%20project\Most_Optimal_Skills_for_Data_Analyst_in_the_US_Technology.png)

*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US with color labels for technology* 

### Insights 

* The scatter plot shows that most programming skills (colored blue) tend to cluster at higher salary levels compared to other categories, indicating that programming expertise may offer greater salary benefits within the data analytics field.

* The database skills (colored orange), such as Oracle and SQL Server, are associated with some of the highest salaries among data analyst tools. This highlights the significant demand and valuation for data management and manipulation expertise in the industry.

* Analyst tools (colored green), including Tableau and Power BI, are prevalent in job postings and offer competitive salaries, indicating that visualization and data analysis software are crucial for current data roles. This category not only offers good salaries but is also versatile across different types of data tasks.

# What I Learned 

Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific lessons I learned:

- **Advanced Python Usage:** Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.

- **Importance of Data Cleaning:** Thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.

- **Strategic Skill Analysis:** The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.

# Insights
This project provided several general insights into the data job market for analysts:

- **Skill Demand and Salary Correlation:** There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.

- **Market Trends:** Changing trends in skill demand highlight the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.

- **Economic Value of Skills:** Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns

# Challenges I Faced

This project was not without its challenges, as I had never coded before, but it provided good learning opportunities:

- **Data Inconsistencies:** Handling missing or inconsistent data entries required careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.

- **Complex Data Visualization:** Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.

- **Balancing Breadth and Depth:** Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details

# Conclusion
This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. 

The insights gained enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. 

As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a solid foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.

