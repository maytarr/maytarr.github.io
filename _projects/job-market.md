---
title: Data Job Market Analysis
description: Which skills do US data analyst jobs ask for most, how did demand change through 2023, and which skills are both in demand and well paid?
date: 2026-01-20
status: Completed
tags: [Python, Pandas, Data Visualisation]
github: https://github.com/maytarr/Python_Data_Project
image: /assets/images/job-market/optimal-skills-by-technology.png
image_alt: Scatter plot of data analyst skills by how often job postings ask for them and their median salary
---

I wanted to understand the data job market before stepping into it: which skills employers actually ask for, and which ones pay. This project analyses 2023 US job postings for data roles to answer that.

It's the capstone project from [Luke Barousse's Python for Data Analytics course](https://www.lukebarousse.com/python), which also provides the dataset. I followed the course's structure and wrote my own analysis and interpretation of the results.

## Key findings

- **SQL is the core skill for data analysts.** It appears in more than half of all data analyst job postings, more than any other skill.
- **Excel is the second most requested skill**, but it pays less than most other skills on the list.
- **Python is the best skill to learn next:** it's in about a third of postings and has the highest median salary (around $97K) of the common skills.
- **Oracle pays the most** (just under $100K median) but appears in only a small share of postings.

## The questions

1. What are the most in-demand skills for the three most popular data roles?
2. How did demand for those skills change for data analysts during 2023?
3. How well do data jobs and skills pay?
4. Which skills are both in high demand and well paid?

## Data and tools

The dataset is a collection of real data job postings from 2023, with job titles, locations, salaries and required skills. I focused on postings in the United States.

```python
from datasets import load_dataset
import pandas as pd
import ast

df = load_dataset('lukebarousse/data_jobs')['train'].to_pandas()

# Convert dates, and turn the skills column from text into Python lists
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)

df_US = df[df['job_country'] == 'United States']
```

I used **Pandas** for the analysis, **Matplotlib** and **Seaborn** for the charts, and **Jupyter Notebooks** to keep code, notes and results together.

## 1. Most in-demand skills by role

I picked the three most common job titles and calculated what share of their postings ask for each skill.

![Bar charts of the top five skills for data analysts, data engineers and data scientists](/assets/images/job-market/skill-demand-by-role.png)
*Share of US job postings that ask for each skill, for the three most common data roles.*

- **SQL leads for data analysts (51%) and data engineers (68%).** For data scientists, Python leads at 72%.
- **Data analysts are expected to know general tools** like Excel (41%) and Tableau (28%).
- **Data engineers need more specialised technical skills**, such as AWS, Azure and Spark.

## 2. How skill demand changed in 2023

Next, I grouped data analyst postings by month to see how demand for the top five skills changed during the year.

![Line chart of monthly demand for SQL, Excel, Python, Tableau and Power BI in 2023](/assets/images/job-market/skill-trend-data-analysts.png)
*Share of US data analyst postings asking for each skill, by month in 2023.*

- **SQL stayed the most requested skill all year**, though its share slipped from about 63% in January to about 53% in December.
- **Excel stayed second**, with a dip between September and November before recovering in December.
- **Python, Tableau and Power BI stayed fairly steady**, with Python rising in the last two months to overtake Tableau.

## 3. Salaries by role and skill

I compared salary distributions for the six most common data job titles.

![Box plots of yearly salaries for the six most common data job titles](/assets/images/job-market/salary-by-role.png)
*Yearly salary distributions for the six most common US data job titles.*

- **Pay rises with seniority and specialisation.** Senior data scientists and senior data engineers have the highest median salaries, at around $150K.
- **Data scientist salaries vary the most**, with a few postings above $500K.
- **Data analysts have the lowest median salary** (around $90K) and a narrower range than the other roles.

Then I focused on data analysts and compared the highest-paid skills with the most in-demand ones.

![Two bar charts: top ten highest-paid skills and top ten most in-demand skills for data analysts](/assets/images/job-market/top-paid-vs-in-demand.png)
*Median salary for the highest-paid skills (top) and the most in-demand skills (bottom) for US data analysts.*

- **The highest-paid skills are specialised and rare**, such as dplyr, Bitbucket and GitLab.
- **The most in-demand skills are foundational**, such as SQL, Excel and PowerPoint, and they pay less.
- In other words, the skills that get you hired and the skills that pay the most are not the same list.

## 4. The best skills to learn

To find skills that are both common and well paid, I plotted each skill's share of data analyst postings against its median salary, and coloured them by type of technology.

![Scatter plot of data analyst skills by share of job postings and median salary, coloured by technology type](/assets/images/job-market/optimal-skills-by-technology.png)
*Skills that appear in more postings sit further right; better-paid skills sit higher.*

- **Python stands out:** it's in about a third of postings and has a median salary of around $97K.
- **SQL is in by far the most postings** (almost 60%), with a median salary around $90K.
- **Tableau** is a strong analyst tool, in about 30% of postings at around $92K.
- **Oracle and SQL Server pay well** but appear in fewer than 10% of postings.
- **Excel, PowerPoint and Word are common but pay the least.**

## What I learned

- **Data cleaning comes first.** Job skills were stored as text and needed converting before I could count them, and missing values had to be handled carefully.
- **Good charts take iteration.** Getting labels, colours and layouts clear enough to read at a glance took longer than the analysis.
- **Demand and pay tell different stories.** Looking at both together is what makes the results useful for planning what to learn.

<!--
TODO (Mayeadeh): add a short section in your own words. This is where you stand out
from other people who did the same course. For example:

## What I'd do next

- Repeat the analysis for South Africa, or compare it with the US
- Use more recent data (2024 or 2025) to see if the trends held
- ...
-->

The full code, notebooks and charts are in the [GitHub repository](https://github.com/maytarr/Python_Data_Project).
