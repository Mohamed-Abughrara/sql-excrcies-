# SQL Project: Data Job Market Analysis

An exploration of the data job market using PostgreSQL, focused on **Data Analyst** roles. The queries look at top-paying jobs, the skills those jobs ask for, the most in-demand skills, and where high demand meets high salary.

The project structure and queries follow Luke Barousse's [SQL_Project_Data_Job_Analysis](https://github.com/lukebarousse/SQL_Project_Data_Job_Analysis).

## Questions

1. What are the top-paying Data Analyst jobs?
2. What skills do those top-paying jobs require?
3. Which skills are most in demand for Data Analysts?
4. Which skills are linked to higher salaries?
5. What are the most optimal skills to learn (high demand and high pay)?

## Tools

- **PostgreSQL**: the database that holds the job postings data
- **SQL**: all analysis is written as plain SQL queries
- **VS Code** with the SQLTools extension: writing and running queries
- **pgAdmin / PSQL Tool**: loading the CSV data
- **Git & GitHub**: version control

## Repository structure

```
├── sql_load/           # Create the database, tables, and load the CSV data
│   ├── 1_create_database.sql
│   ├── 2_create_tables.sql
│   └── 3_modify_tables.sql
├── project_sql/        # The five analysis queries (one per question)
│   ├── 1_top_paying_jobs.sql
│   ├── 2_top_paying_job_skills.sql
│   ├── 3_top_demanded_skills.sql
│   ├── 4_top_paying_skills.sql
│   └── 5_optimal_skills.sql
├── advanced_sql/       # Practice queries: tables, dates, CASE, subqueries/CTEs, UNION
└── csv_files/          # Data files (not committed; see .gitignore)
```

## Data model

| Table | Description | Key |
|---|---|---|
| `company_dim` | Companies posting jobs | `company_id` |
| `skills_dim` | Skills and their type (e.g. programming, cloud) | `skill_id` |
| `job_postings_fact` | Job postings: title, location, schedule, remote flag, salary, posted date | `job_id` → `company_id` |
| `skills_job_dim` | Which skills each job asks for (bridge table) | `job_id` + `skill_id` |

## Setup

1. Put the four CSV files in `csv_files/`, named after their tables:
   `company_dim.csv`, `skills_dim.csv`, `job_postings_fact.csv`, `skills_job_dim.csv`.
   The files are **tab-delimited**.
2. Run [`sql_load/1_create_database.sql`](sql_load/1_create_database.sql) to create the `sql_course` database.
3. Connect to `sql_course` and run [`sql_load/2_create_tables.sql`](sql_load/2_create_tables.sql).
4. Run [`sql_load/3_modify_tables.sql`](sql_load/3_modify_tables.sql) to load the data. Load order matters because of foreign keys:
   `company_dim` → `skills_dim` → `job_postings_fact` → `skills_job_dim`.
   - If `COPY` gives *permission denied*, use the `\copy` commands in the comment at the top of that file in pgAdmin's PSQL Tool instead, and update the file paths to match where your CSV files are.
5. Run the queries in [`project_sql/`](project_sql/).

## The analysis

| # | Query | What it answers |
|---|---|---|
| 1 | [Top-paying jobs](project_sql/1_top_paying_jobs.sql) | The 10 highest-paying remote Data Analyst postings with a listed salary, with company names |
| 2 | [Skills for top-paying jobs](project_sql/2_top_paying_job_skills.sql) | The skills required by those 10 postings |
| 3 | [In-demand skills](project_sql/3_top_demanded_skills.sql) | The 5 skills that appear in the most Data Analyst postings |
| 4 | [Top-paying skills](project_sql/4_top_paying_skills.sql) | The skills with the highest average salary for Data Analyst roles |
| 5 | [Optimal skills](project_sql/5_optimal_skills.sql) | Skills that are both in high demand and well paid, for remote roles |

## Results

_To be added after running the queries on this dataset._

## What I learned

- **Joins and data modeling**: combining fact and dimension tables to link jobs, companies, and skills
- **Aggregation**: `COUNT`, `AVG`, and `GROUP BY` to measure demand and salary by skill
- **CTEs and subqueries**: breaking multi-step questions into readable building blocks
- **Data loading**: creating tables with keys and importing CSV files with `COPY` / `\copy`
