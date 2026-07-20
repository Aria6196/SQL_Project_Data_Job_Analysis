# Introduction
Explore the data job market through an analysis of data analyst and data scientist roles focusing on remote jobs. This project examines the highest-paying positions, the most sought-after skills, and the overlap between salary and demand to uncover valuable insights into today's data careers.

SQL queries for this project are available here: [project_sql](/project_sql/)

# Background
This project was created to better understand the data analyst and data scientist job market by identifying high-paying opportunities and the skills employers value most. The goal is to provide a streamlined resource for anyone looking to focus on the most rewarding and in-demand career paths.

The dataset used in this analysis comes from this [SQL Course](https://lukebarousse.com/sql), which includes detailed information on job titles, salaries, locations, and required technical skills.

### The questions I wanted to answer through my SQL queries were:
1. What are the top-paying data analyst and scientist jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for data analysts and scientists?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn

# Tools I Used
To explore the data analyst and data scientist job market, I relied on the following tools::

- **SQL**: Used to query the database and extract meaningful insights from the job posting data.
- **PostgreSQL**: Served as the database management system for storing and querying the dataset efficiently.
- **Visual Studio Code**: My primary development environment for writing and executing SQL queries.
- **Git & GitHub**: Used for version control, project management, and sharing the SQL scripts and analysis.

# The Analysis
Each SQL query was designed to investigate a specific aspect of the data job market. Below is a breakdown of the approach taken for each analysis.

### 1. Top Paying Data Analyst and Scientist Jobs
To identify the highest-paying data analyst and data scientist positions, I filtered job postings based on annual salary availability and remote ("Anywhere") locations. This analysis highlights some of the most lucrative opportunities available in each career path.

#### Query for Top Paying Data Analyst Roles
```sql
SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    company_dim.name AS company_name
FROM job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst' AND 
    job_location = 'Anywhere' AND
    salary_year_avg IS NOT NULL
ORDER BY salary_year_avg DESC
LIMIT 10;
```
![Top Paying Data Analyst Roles](assets/Top%2010%20Highest-Paying%20Data%20Analyst%20Jobs%20(2023).png)

Breakdown of the top data analyst jobs in 2023:
- **Broad Salary Range:** The top 10 highest-paying data analyst positions offer salaries ranging from $184,000 to $650,000, demonstrating the strong earning potential available in the field.
- **Employers Across Multiple Industries:** Organizations such as SmartAsset, Meta, and AT&T are among the companies offering these competitive salaries, reflecting the widespread demand for data analysts across different sectors.
- **Wide Range of Job Titles:** The results include roles spanning from Data Analyst to Director of Analytics, illustrating the diverse career progression and specialization opportunities within data analytics.


#### Query for Top Paying Data Scientist Roles
```sql
SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    company_dim.name AS company_name
FROM job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE job_title_short = 'Data Scientist' AND
    job_location = 'Anywhere' AND
    salary_year_avg IS NOT NULL
ORDER BY salary_year_avg DESC
LIMIT 10;
```

![Top Paying Data Analyst Roles](assets/Top%2010%20Highest-Paying%20Data%20Scientist%20Jobs%20(2023).png)

Breakdown of the top data scientist jobs in 2023:
- **Exceptional Salary Potential:** The top 10 highest-paying roles offer salaries ranging from $300,000 to $550,000, with an average of approximately $365,850.
- **Leadership & Specialized Roles Dominate:** Leadership positions such as Head of Data Science, Director, Principal, and Staff Data Scientist dominate the highest-paying jobs.
- **High-Paying Opportunities Across Industries:** Companies like Selby Jennings, Algo Capital Group, and Demandbase offer top salaries, reflecting strong demand for data science talent across finance and technology.


### 2. Skills for Top Paying Data Analyst and Scientist Jobs
To understand what skills are required for these jobs, I joined the job postings with the skills data, providing insights into what employers value for high-compensation roles.

Query for Top Skills for Data Analyst Roles
```sql
WITH top_paying_jobs_analyst AS (
    SELECT 
        job_id,
        job_title,
        salary_year_avg,
        company_dim.name AS company_name
    FROM 
        job_postings_fact
        LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE 
        job_title_short = 'Data Analyst' AND 
        salary_year_avg IS NOT NULL AND
        job_location = 'Anywhere'
    ORDER BY salary_year_avg DESC
    LIMIT 10
)

SELECT 
    top_paying_jobs_analyst.*,
    skills_dim.skills
FROM top_paying_jobs_analyst
    INNER JOIN skills_job_dim ON top_paying_jobs_analyst.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY salary_year_avg DESC;
```

![Top Skills for Data Analyst Roles](assets/Top%20Skills%20Required%20for%20Highest-Paying%20Data%20Analyst%20Jobs%20(2023).png)

Breakdown of the top data analyst skills in 2023:
- **SQL Dominates the Top-Paying Roles:** 8 out of 10 of the highest-paying data analyst job postings require SQL, making it the most frequently requested skill among top-paying positions.
- **Python is Nearly Universal:** Python appears in 7 out of 10 job postings, reinforcing its position as the core programming language for high-paying data science roles.
- **Visualization and Data Engineering Skills are Highly Valued:** Tableau is listed in 6 postings, while R appears in 4, and both Pandas and Snowflake appear in 3 postings each, showing that employers seek candidates with expertise in data visualization, analysis, and modern cloud data platforms.


Query for Top Skills for Data Scientist Roles
```sql
WITH top_paying_jobs_scientist AS (
    SELECT 
        job_id,
        job_title,
        salary_year_avg,
        company_dim.name AS company_name
    FROM 
        job_postings_fact
        LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE 
        job_title_short = 'Data Scientist' AND 
        salary_year_avg IS NOT NULL AND
        job_location = 'Anywhere'
    ORDER BY salary_year_avg DESC
    LIMIT 10
)

SELECT 
    top_paying_jobs_scientist.*,
    skills_dim.skills
FROM top_paying_jobs_scientist
    INNER JOIN skills_job_dim ON top_paying_jobs_scientist.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY salary_year_avg DESC;
```

![Top Skills for Data Scientist Roles](assets/Top%20Skills%20Required%20for%20Highest-Paying%20Data%20Scientist%20Jobs%20(2023).png)

Breakdown of the top data scientist skills in 2023:
- **Python is the Most Valued Skill:** Python appears in 5 of the top 10 highest-paying data scientist job postings, making it the most valuable technical skill.
- **SQL and Cloud Technologies are Essential:** SQL (4) and AWS (3) are frequently requested, emphasizing the importance of database and cloud expertise.
- **Machine Learning Expertise is Highly Desired:** Frameworks such as TensorFlow, Keras, PyTorch, and Scikit-learn highlight the demand for machine learning skills.
- **Diverse Technical Stack:** Employers seek experience in big data, cloud platforms, visualization tools, and data science libraries, reflecting the diverse responsibilities of data scientists.

### 3. In-Demand Skills for Data Analyst and Scientist
This query helped identify the skills most frequeantly requested in job postings, directing focus to areas with high demand.

Query for Most In-Demanded Skills for Data Analyst Roles
```sql
-- DATA ANALYST
SELECT
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM
    job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
GROUP BY skills
ORDER BY demand_count DESC
LIMIT 5;
```

Most In-Demand Skills for Data Analyst Roles
| Skill | Job Postings |
|:------|-------------:|
| SQL | 92,628 |
| Excel | 67,031 |
| Python | 57,326 |
| Tableau | 46,554 |
| Power BI | 39,468 |

Breakdown of the most in-demand data analyst skills in 2023:
- **SQL Leads the Market:** With 92,628 job postings, SQL is the most sought-after skill, making it essential for aspiring data analysts.
- **Excel Remains Essential:** Excel (67,031) continues to be a core requirement, highlighting its importance in data analysis and reporting.
- **Python is Increasingly Valuable:** Appearing in 57,326 postings, Python reflects the growing demand for automation and advanced analytics.
- **Visualization Skills Matter:** Tableau (46,554) and Power BI (39,468) are highly requested, emphasizing the importance of presenting data through interactive dashboards.

Query for Most In-Demanded Skills for Data Scientist Roles
```sql
-- DATA SCIENTIST
SELECT
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Scientist'
GROUP BY skills
ORDER BY demand_count DESC
LIMIT 5;
```

Most In-Demand Skills for Data Scientist Roles
| Skill | Job Postings |
|:------|-------------:|
| Python | 114,016 |
| SQL | 79,174 |
| R | 59,754 |
| SAS | 29,642 |
| Tableau | 29,513 |

Breakdown of the most in-demand data scientist skills in 2023:
- **Python Dominates Demand:** With 114,016 job postings, Python is the most sought-after skill for data scientist roles.
- **SQL Remains Essential:** SQL (79,174) continues to be a core requirement for working with large datasets and databases.
- **R is Still Widely Used:** Appearing in 59,754 postings, R remains an important language for statistical analysis and data science.
- **Analytics and Visualization Matter:** SAS (29,642) and Tableau (29,513) are frequently requested, highlighting the value of statistical analysis and data visualization skills.

### 4. Skills Based on Salary
Exploring the average salaries associated with different skills revealed which skils are the highest paying.

Query for Highest-Paying Skills for Data Analyst Roles
```sql
SELECT
    skills,
    ROUND(AVG(salary_year_avg), 0) AS average_salary
FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' AND
    salary_year_avg IS NOT NULL
GROUP BY skills
ORDER BY average_salary DESC
LIMIT 25;
```

Highest-Paying Data Analyst Skills in 2023:
| Skill | Average Salary (USD) |
|:----------------|--------------------:|
| SVN | $400,000 |
| Solidity | $179,000 |
| Couchbase | $160,515 |
| DataRobot | $155,486 |
| Golang | $155,000 |
| MXNet | $149,000 |
| dplyr | $147,633 |
| VMware | $147,500 |

| Terraform | $146,734 |
| Twilio | $138,500 |
| GitLab | $134,126 |
| Kafka | $129,999 |
| Puppet | $129,820 |
| Keras | $127,013 |
| PyTorch | $125,226 |
| Perl | $124,686 |
| Ansible | $124,370 |
| Hugging Face | $123,950 |
| TensorFlow | $120,647 |
| Cassandra | $118,407 |
| Notion | $118,092 |
| Atlassian | $117,966 |
| Bitbucket | $116,712 |
| Airflow | $116,387 |
| Scala | $115,480 |

Breakdown of the highest-paying data analyst skills in 2023:
- **Specialized Skills Command Premium Salaries:** SVN ($400,000) tops the list, followed by niche technologies such as Solidity and Couchbase.
- **AI & Machine Learning Skills Pay Well:** Tools like DataRobot, PyTorch, TensorFlow, Keras, and Hugging Face rank among the highest-paying skills.
- **Data Engineering is Highly Valued:** Technologies including Kafka, Airflow, Cassandra, and Terraform highlight the demand for scalable data infrastructure expertise.
- **Niche Skills Offer Competitive Salaries:** Many of the top-paying skills are specialized technologies that appear less frequently but command significantly higher compensation.

Query for Highest-Paying Skills for Data Scientist Roles
```sql
SELECT
    skills,
    ROUND(AVG(salary_year_avg), 0) AS average_salary
FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Scientist' AND
    salary_year_avg IS NOT NULL
GROUP BY skills
ORDER BY average_salary DESC
LIMIT 25;
```

| Skill | Average Salary (USD) |
|:----------------|--------------------:|
| Asana | $215,477 |
| Airtable | $201,143 |
| Red Hat | $189,500 |
| Watson | $187,417 |
| Elixir | $170,824 |
| Lua | $170,500 |
| Slack | $168,219 |
| Solidity | $166,980 |
| Ruby on Rails | $166,500 |
| RShiny | $166,436 |
| Notion | $165,636 |
| Objective-C | $164,500 |
| Neo4j | $163,971 |
| dplyr | $163,111 |
| Hugging Face | $160,868 |
| DynamoDB | $160,581 |
| Haskell | $157,500 |
| Unity | $156,881 |
| Airflow | $155,878 |
| CodeCommit | $154,684 |
| Unreal | $153,278 |
| Theano | $153,133 |
| Zoom | $151,677 |
| BigQuery | $149,292 |
| Atlassian | $148,715 |

Breakdown of the highest-paying data scientist skills in 2023:
- **Productivity Platforms Top the List:** Asana ($215,477) and Airtable ($201,143) are associated with the highest average salaries.
- **AI & ML Skills Remain Valuable:** Skills such as Hugging Face, Theano, and dplyr reflect the strong demand for machine learning expertise.
- **Cloud & Big Data Knowledge Pays More:** Technologies like DynamoDB, BigQuery, Airflow, and Neo4j are linked to high-paying data science roles.
- **pecialized Technologies Earn Premium Salaries:** Many top-paying skills are niche tools and frameworks that are commonly required in advanced data science positions.

### 5. Optimal Skills to Learn
Exploring the average salaries associated with the most in-demand skills revealed which skils are optimal to learning.

Query for Most Optimal Skills to Learn for Data Analyst Roles
```sql
SELECT
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 2) AS avg_salary
FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skillS_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' AND
    salary_year_avg IS NOT NULL
GROUP BY skills_dim.skill_id
HAVING COUNT(skills_job_dim.job_id) > 10
ORDER BY demand_count DESC,
    avg_salary DESC
LIMIT 25;
```
| Skill | Demand Count | Average Salary (USD) |
|:--------------|-------------:|---------------------:|
| SQL | 3,083 | $96,435 |
| Excel | 2,143 | $86,419 |
| Python | 1,840 | $101,512 |
| Tableau | 1,659 | $97,978 |
| R | 1,073 | $98,708 |
| Power BI | 1,044 | $92,324 |
| Word | 527 | $82,941 |
| PowerPoint | 524 | $88,316 |
| SAS | 500 | $93,707 |
| SQL Server | 336 | $96,191 |
| Oracle | 332 | $100,964 |
| Azure | 319 | $105,400 |
| AWS | 291 | $106,440 |
| Go | 288 | $97,267 |
| Flow | 271 | $98,020 |
| Looker | 260 | $103,855 |
| Snowflake | 241 | $111,578 |
| SPSS | 212 | $85,293 |
| Spark | 187 | $113,002 |
| VBA | 185 | $93,845 |
| SAP | 183 | $92,446 |
| Outlook | 180 | $80,680 |
| SharePoint | 174 | $89,027 |
| Google Sheets | 155 | $84,130 |

Breakdown of the most optimal data analyst skills in 2023:
- **SQL is the Best Overall Skill:** SQL has the highest demand (3,083 postings) while maintaining a strong average salary of $96,435.
- **Python Offers High Value:** Python combines high demand (1,840) with an average salary exceeding $100,000, making it one of the most rewarding skills.
- **Cloud Skills Pay More:** AWS ($106,440), Azure ($105,400), Snowflake ($111,578), and Spark ($113,002) offer some of the highest salaries despite lower demand.
- **Balance Demand and Salary:** Skills like SQL, Python, Tableau, and Power BI provide the best combination of strong market demand and competitive salaries.

Query for Most Optimal Skills to Learn for Data Scientist Roles
```sql
SELECT
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 2) AS avg_salary
FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skillS_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Scientist' AND
    salary_year_avg IS NOT NULL
GROUP BY skills_dim.skill_id
HAVING COUNT(skills_job_dim.job_id) > 10
ORDER BY demand_count DESC,
    avg_salary DESC
LIMIT 25;
```

| Skill | Demand Count | Average Salary (USD) |
|:----------------|-------------:|---------------------:|
| Python | 4,312 | $138,049 |
| SQL | 3,151 | $138,431 |
| R | 2,486 | $135,165 |
| Tableau | 1,278 | $131,636 |
| AWS | 1,016 | $138,861 |
| Spark | 946 | $144,399 |
| TensorFlow | 641 | $143,440 |
| Azure | 623 | $132,897 |
| Excel | 617 | $124,593 |
| SAS | 615 | $122,910 |
| Hadoop | 602 | $136,429 |
| PyTorch | 564 | $145,989 |
| Java | 557 | $130,701 |
| Power BI | 489 | $118,603 |
| Pandas | 481 | $138,269 |
| Scikit-learn | 392 | $141,777 |
| Scala | 381 | $145,056 |
| Git | 376 | $123,277 |
| NumPy | 339 | $136,720 |
| Go | 316 | $147,466 |
| Databricks | 314 | $135,491 |
| Snowflake | 313 | $142,691 |
| C++ | 290 | $134,698 |
| C | 280 | $142,278 |

# Conclusions
### Insights

1. **Top-Paying Jobs**
    
    - The highest-paying remote data analyst roles offer salaries up to **$650,000**, demonstrating the strong earning potential of senior analytics positions.
    - Remote data scientist roles offer salaries reaching **$550,000**, highlighting the high market value of expertise in machine learning and advanced analytics.

2. **Skills for Top-Paying Jobs**

    - High-paying data analyst positions frequently require **SQL**, emphasizing its importance for querying, managing, and analyzing business data.
    - Top-paying data scientist positions are strongly centered around **Python**, reflecting its widespread use in machine learning, automation, and AI development.

3. **Most In-Demand Skills**
    
    - **SQL** is the most requested skill in data analyst job postings, making it the foundation of a successful analytics career.
    - **Python** dominates data scientist job postings, reinforcing its role as the primary programming language for modern data science.

4. **Skills with Higher Salaries**

    - Specialized tools such as **SVN** and **Solidity** are associated with the highest average salaries for data analysts, suggesting employers pay a premium for niche technical expertise.
    - Skills such as **Asana** and **Airtable** rank among the highest-paying for data scientists, indicating that expertise in collaborative platforms and specialized technologies can significantly increase earning potential.

5. **Optimal Skills for Job Market Value**

    - **SQL** offers the strongest balance of demand and salary, making it one of the most valuable skills for aspiring data analysts.
    - **Python** combines the highest demand with competitive salaries, making it the most strategic skill for data scientists seeking long-term career growth.

### Closing Thoughts

Working on this project gave me valuable insights into the world of data careers. Using SQL for the first time as a data analytics tool rather than solely as a database management language significantly strengthened my SQL skills. Analyzing the data also helped me identify the technical skills that are most valuable for building a successful career in the data field.

Before starting this project, I believed that Python was the only essential language for data analytics. However, this experience showed me that SQL is equally important and serves as the foundation for extracting and analyzing data efficiently. More importantly, the project challenged me to think like a data analyst rather than just a technical developer, an aspect that is often overlooked.

Overall, this exploration reinforced the importance of continuous learning, adaptability, and developing both technical and analytical thinking to succeed in the ever-evolving field of data. 