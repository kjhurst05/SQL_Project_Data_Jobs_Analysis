# Introduction

This is a dive into the 2023 data job market. I want to thank Luke Barousse for offering his SQL course on YouTube (https://www.youtube.com/watch?v=7mz73uXD9DA&t=12640s). He has taught an incredible course about how to use SQL for data driven insights and decisions. This showcases my learning of SQL's syntax. 

SQL queries? Check them out here [project_sql](/project_sql/)
# Background

### These were the questions that I wanted to answer through my SQL queries:

1. What are the top paying data analyst jobs in Texas?
2. What are the skills required for these top-paying jobs?
3. What skills are most in demand for data analysts?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn?
6. What are most used skills for the top paying data analyst jobs in Texas.
# Tools I Used

In order to answer these questions, I have used four tools for this analysis.

- **SQL:** The backbone of my analysis, allowing me to query the database and make critical insights
- **PostgreSQL:** The chosen database management system, ideal for handling the job posting data.
- **Visual Studio Code:** My go_to for database management and executing SQL queries
- **Git & Github:** Essential for version control and sharing my SQL scripts and analysis, ensuring collaboration and project tracking.

# The Analysis

### 1. Top Paying Data Analyst Jobs in Texas
To identify the highest_paying roels, I filtered data analyst positions by average yearly salary and location. This query highlights the high paying opportunities in the field.

```sql
SELECT
    job_id,
    company_dim.name AS company_name,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date
FROM
    job_postings_fact
LEFT JOIN company_dim 
    ON company_dim.company_id = job_postings_fact.company_id
WHERE
    job_title_short = 'Data Analyst' AND
    job_location LIKE '%TX' AND
    salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT 
    9;
```

| Company         | Job Title                                         | Salary   |
| --------------- | ------------------------------------------------- | -------- |
| Torc Robotics   | Director of Safety Data Analysis                  | $375,000 |
| Care.com        | Head of Data Analytics                            | $350,000 |
| Google          | Partner Technology Manager, Data Analytics and AI | $254,000 |
| Upstart         | Staff Data Analyst, Credit Analytics              | $200,000 |
| Hitachi Vantara | Data Analytics Delivery Manager/Lead              | $185,000 |

These are simply the top 5 paying jobs in Texas on 2023

### 2. Skills for Top Paying Jobs Data Analyst Jobs

To identify the skills for highest_paying roles, I used a JOIN between the skills table and job postings table. This query highlights the high paying skills in the field.


```sql
WITH top_paying_jobs AS (
    SELECT
        job_id,
        name AS company_name,
        job_title,
        salary_year_avg
    FROM
        job_postings_fact
    LEFT JOIN company_dim 
        ON company_dim.company_id = job_postings_fact.company_id
    WHERE
        job_title_short = 'Data Analyst' AND
        job_location LIKE '%TX' AND
        salary_year_avg IS NOT NULL
    ORDER BY
        salary_year_avg DESC
    LIMIT 9
)

SELECT
    top_paying_jobs.*,
    skills
FROM
    top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
```

| Job ID | Company Name | Job Title | Salary Year Avg | Skills |
| --- | --- | --- | --- | --- |
| 229253 | Torc Robotics | Director of Safety Data Analysis | 375000.0 | sql |
| 229253 | Torc Robotics | Director of Safety Data Analysis | 375000.0 | python |
| 229253 | Torc Robotics | Director of Safety Data Analysis | 375000.0 | r |
| 229253 | Torc Robotics | Director of Safety Data Analysis | 375000.0 | sas |
| 229253 | Torc Robotics | Director of Safety Data Analysis | 375000.0 | matlab |
| 229253 | Torc Robotics | Director of Safety Data Analysis | 375000.0 | spark |
| 229253 | Torc Robotics | Director of Safety Data Analysis | 375000.0 | airflow |
| 229253 | Torc Robotics | Director of Safety Data Analysis | 375000.0 | excel |
| 229253 | Torc Robotics | Director of Safety Data Analysis | 375000.0 | tableau |
| 229253 | Torc Robotics | Director of Safety Data Analysis | 375000.0 | power bi |
| 229253 | Torc Robotics | Director of Safety Data Analysis | 375000.0 | sas |
| 101757 | Care.com | Head of Data Analytics | 350000.0 | sql |
| 101757 | Care.com | Head of Data Analytics | 350000.0 | python |
| 101757 | Care.com | Head of Data Analytics | 350000.0 | r |
| 101757 | Care.com | Head of Data Analytics | 350000.0 | bigquery |
| 101757 | Care.com | Head of Data Analytics | 350000.0 | snowflake |
| 101757 | Care.com | Head of Data Analytics | 350000.0 | tableau |
| 101757 | Care.com | Head of Data Analytics | 350000.0 | power bi |
| 101757 | Care.com | Head of Data Analytics | 350000.0 | looker |
| 285766 | Google | Partner Technology Manager, Data Analytics and AI | 254000.0 | gcp |
| 1281297 | Upstart | Staff Data Analyst, Credit Analytics | 200000.0 | python |
| 1281297 | Upstart | Staff Data Analyst, Credit Analytics | 200000.0 | r |
| 1281297 | Upstart | Staff Data Analyst, Credit Analytics | 200000.0 | sas |
| 1281297 | Upstart | Staff Data Analyst, Credit Analytics | 200000.0 | vba |
| 1281297 | Upstart | Staff Data Analyst, Credit Analytics | 200000.0 | databricks |
| 1281297 | Upstart | Staff Data Analyst, Credit Analytics | 200000.0 | redshift |
| 1281297 | Upstart | Staff Data Analyst, Credit Analytics | 200000.0 | snowflake |
| 1281297 | Upstart | Staff Data Analyst, Credit Analytics | 200000.0 | excel |
| 1281297 | Upstart | Staff Data Analyst, Credit Analytics | 200000.0 | tableau |
| 1281297 | Upstart | Staff Data Analyst, Credit Analytics | 200000.0 | looker |
| 1281297 | Upstart | Staff Data Analyst, Credit Analytics | 200000.0 | sas |
| 1730160 | Hitachi Vantara | BFSI Data Analytics Delivery Manager/Lead | 185000.0 | azure |
| 1730160 | Hitachi Vantara | BFSI Data Analytics Delivery Manager/Lead | 185000.0 | aws |
| 1730160 | Hitachi Vantara | BFSI Data Analytics Delivery Manager/Lead | 185000.0 | snowflake |
| 1730160 | Hitachi Vantara | BFSI Data Analytics Delivery Manager/Lead | 185000.0 | gcp |
| 325561 | Renuity | Director of Consumer Data and Analytics | 185000.0 | sql |
| 325561 | Renuity | Director of Consumer Data and Analytics | 185000.0 | mysql |
| 325561 | Renuity | Director of Consumer Data and Analytics | 185000.0 | sql server |
| 325561 | Renuity | Director of Consumer Data and Analytics | 185000.0 | azure |
| 325561 | Renuity | Director of Consumer Data and Analytics | 185000.0 | aws |
| 325561 | Renuity | Director of Consumer Data and Analytics | 185000.0 | tableau |
| 325561 | Renuity | Director of Consumer Data and Analytics | 185000.0 | looker |
| 325561 | Renuity | Director of Consumer Data and Analytics | 185000.0 | qlik |
| 985088 | Meta | Privacy Data Analyst | 157500.0 | sql |
| 985088 | Meta | Privacy Data Analyst | 157500.0 | python |
| 985088 | Meta | Privacy Data Analyst | 157500.0 | r |
| 985088 | Meta | Privacy Data Analyst | 157500.0 | php |

There are a lot of commonalities here. Here are the most highlighted skills of these jobs (more concise on question 6):

- SQL
- Tableau
- Python
- R
- SAS




### 3. Skills in Demand for Data Analyst Jobs

To identify the skills in demand, I used the GROUP BY statement to gather the skills and their respective counts. This query highlights the demand of skills in the field.



```sql
SELECT
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM 
    job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5
```

| Skill    | Demand Count |
| -------- | ------------ |
| SQL      | 92,628       |
| Excel    | 67,031       |
| Python   | 57,326       |
| Tableau  | 46,554       |
| Power BI | 39,468       |


### 4. Top Paying Skills for Data Analyst Jobs

To identify the top paying skills, I JOIN two tables together in order to find the skills and the pay for those skills. This query highlights the high paying skills in the field.

```sql
SELECT
    skills,
    ROUND (AVG(salary_year_avg), 0)  AS skills_to_salary
FROM 
    job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' AND
    salary_year_avg IS NOT NULL 
GROUP BY
    skills
ORDER BY
    skills_to_salary DESC
LIMIT 25
```

| Skills | Salary |
| --- | --- |
| svn | 400000 |
| solidity | 179000 |
| couchbase | 160515 |
| datarobot | 155486 |
| golang | 155000 |
| mxnet | 149000 |
| dplyr | 147633 |
| vmware | 147500 |
| terraform | 146734 |
| twilio | 138500 |
| gitlab | 134126 |
| kafka | 129999 |
| puppet | 129820 |
| keras | 127013 |
| pytorch | 125226 |
| perl | 124686 |
| ansible | 124370 |
| hugging face | 123950 |
| tensorflow | 120647 |
| cassandra | 118407 |
| notion | 118092 |
| atlassian | 117966 |
| bitbucket | 116712 |
| airflow | 116387 |
| scala | 115480 |



### 5. Optimal Skills for Data Analyst Jobs

To identify the top paying skills, I combined the demand of skills and top paying skills to find the most optimal skills in the market. This query highlights the most optimal skills in the field.

```sql
WITH skills_demand AS (
    SELECT
        skills_dim.skill_id,
        skills_dim.skills,
        COUNT(skills_job_dim.job_id) AS demand_count
    FROM 
        job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE
        job_title_short = 'Data Analyst' AND
        salary_year_avg IS NOT NULL 
    GROUP BY
        skills_dim.skill_id
), average_salary AS (
    SELECT
        skills_dim.skill_id,
        ROUND (AVG(salary_year_avg), 0) AS skills_to_salary
    FROM 
        job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE
        job_title_short = 'Data Analyst' AND
        salary_year_avg IS NOT NULL 
    GROUP BY
        skills_dim.skill_id
)

SELECT
    skills_demand.skill_id,
    skills_demand.skills,
    demand_count,
    skills_to_salary
FROM
    skills_demand
INNER JOIN average_salary ON skills_demand.skill_id = average_salary.skill_id
WHERE
    demand_count > 10
ORDER BY
    demand_count DESC,
    skills_to_salary DESC
```

| Skill     | Demand Count | Avg Salary |
| --------- | ------------ | ---------- |
| SQL       | 3,083        | $96,435    |
| Python    | 1,840        | $101,512   |
| Tableau   | 1,659        | $97,978    |
| Power BI  | 1,044        | $92,324    |
| Snowflake | 241          | $111,578   |

### 6. Count of Skills for Data Analyst Jobs

To identify the top paying skills, I JOIN two tables together in order to find the skills and the pay for those skills. This query highlights the high paying skills in the field.

```sql
WITH top_paying_jobs AS (
    SELECT
        job_id,
        name AS company_name,
        job_title,
        salary_year_avg
    FROM
        job_postings_fact
    LEFT JOIN company_dim 
        ON company_dim.company_id = job_postings_fact.company_id
    WHERE
        job_title_short = 'Data Analyst' AND
        job_location LIKE '%TX' AND
        salary_year_avg IS NOT NULL
    ORDER BY
        salary_year_avg DESC
    LIMIT 9
)

SELECT
    skills,
    COUNT(skills) AS count_of_skills
FROM
    top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
GROUP BY 
    skills
ORDER BY 
    COUNT(skills) DESC
```

| Skills | Count of Skills |
| --- | --- |
| sas | 4 |
| python | 4 |
| r | 4 |
| tableau | 4 |
| sql | 4 |
| looker | 3 |
| snowflake | 3 |
| power bi | 2 |
| gcp | 2 |
| aws | 2 |
| azure | 2 |
| excel | 2 |
| vba | 1 |
| bigquery | 1 |
| databricks | 1 |
| matlab | 1 |
| mysql | 1 |
| php | 1 |
| qlik | 1 |
| redshift | 1 |
| spark | 1 |
| sql server | 1 |
| airflow | 1 |

# What I Learned

Through this experience, here are three major aspects of SQL that I learned:

1. **CTEs and Subqueries:** Learning CTEs (Common Table Expressions) and subqueries is important because they allow me to break complex SQL queries into smaller, more manageable sections. This makes queries easier to read, understand, troubleshoot, and modify. These techniques are especially useful when working with larger datasets or when multiple steps are needed to answer a question.
2. **Data Aggregation:** Learning data aggregation is important because it allows me to summarize large amounts of data into meaningful information. Functions such as COUNT(), SUM(), AVG(), MIN(), and MAX(), along with GROUP BY, can help identify trends, compare groups, and understand patterns within a dataset. This is an important skill for turning raw data into useful insights.
3. **Syntax to Questions:** Learning how to translate questions into SQL syntax is important because SQL is ultimately a tool for answering questions with data. Instead of simply knowing individual SQL commands, I need to understand what information I am looking for and determine how to structure a query to retrieve it. This skill helps me approach real-world data problems logically and efficiently.

# Conclusions
Overall, this experience helped me develop a better understanding of how SQL can be used to work with and analyze data. I learned that SQL is more than just memorizing commands and syntax; it requires understanding the question being asked and determining the best way to use data to answer it. Learning about CTEs and subqueries, data aggregation, and translating questions into SQL queries gave me a stronger foundation for solving more complex data problems. Although there is still more for me to learn, this experience increased my confidence in using SQL and showed me how important it is for turning raw data into meaningful insights.