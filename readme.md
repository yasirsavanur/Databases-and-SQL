# Databases and SQL – Mini Projects

This repo collects three small database projects I worked on while learning SQL and relational databases:

- **Case Study: City of Chicago Data Portal**
- **Human Resources Database**
- **Pet Sale Database**

All of them focus on different parts of the SQL story: designing tables, loading data, writing queries, and in the Chicago case, running SQL directly from a Jupyter Notebook.

---

## Working with Databases from Jupyter

For the Chicago case study I didn’t just write SQL in a database console. Instead, I ran SQL *inside* a Jupyter notebook.

The basic idea:

1. Install the needed packages for database connectivity:

   ```bash
   pip install sqlalchemy==1.3.9 ibm_db_sa
   ```

2. Load the SQL magic extension in the notebook:

   ```python
   %load_ext sql
   ```

3. Connect to the Db2 on Cloud database using a connection string:

   ```python
   %sql ibm_db_sa://username:password@hostname:port/database
   ```

4. From that point on, any cell starting with `%%sql` can run pure SQL:

   ```sql
   %%sql
   SELECT * FROM CHICAGO_SCHOOLS
   FETCH FIRST 5 ROWS ONLY;
   ```

This setup let me:

- Keep **Python** and **SQL** side by side in one notebook  
- Write text, comments, and explanations as Markdown between queries  
- Re-run queries and tweak them easily during exploration  

Exactly the same approach can be used for the other projects too (HR and Pet Sale): define tables in the database, connect from Jupyter, and then run queries with `%%sql` cells while using Python for plotting or extra analysis if needed.

---

## Project 1 – Case Study: City of Chicago Data Portal

Folder: [`Case Study: City of Chicago Data Portal`](Case%20Study%3A%20City%20of%20Chicago%20Data%20Portal)

Main notebook:

- [`Chicago Dataset.ipynb`](Case%20Study%3A%20City%20of%20Chicago%20Data%20Portal/Chicago%20Dataset.ipynb)

Extra documentation:

- [`README.md`](Case%20Study%3A%20City%20of%20Chicago%20Data%20Portal/README.md)

### Backstory

This was a more realistic project: working with **open data** from the City of Chicago.  
The goal was to build a small analytical database in Db2 using three public datasets:

- **Socioeconomic Indicators** – hardship index, per‑capita income, unemployment, education, etc. by community area  
- **Public Schools** – enrollment, school type, safety, health, and environment ratings  
- (Typically) **Crime or related civic data** that can be linked by community area or location  

The assignment walked through the full flow:

1. Download CSV files from Chicago’s Data Portal  
2. Load them into Db2 tables from Jupyter  
3. Write SQL queries to answer specific questions about Chicago communities, schools, and hardship levels  

### Tools and Technologies

- **Database:** IBM Db2 on Cloud  
- **Interface:** Jupyter Notebook (`Chicago Dataset.ipynb`)  
- **SQL Extension:** `%load_ext sql` with the `ibm_db_sa` / `sqlalchemy` connector  
- **Language:** Standard SQL (Db2 flavor)

### What I did in this project

Inside the notebook, I:

- **Explored the structure** of each dataset (columns, data types, keys such as community area number).  
- **Created three Db2 tables** for the Chicago datasets.  
- **Loaded CSV data** from the local environment into those tables.  
- Wrote SQL queries to answer assignment-style questions such as:
  - Which community areas have the **highest hardship index**?  
  - How do **school safety or environment ratings** vary across communities?  
  - How do socioeconomic indicators relate to where schools are located?  

Each query was run from a Jupyter cell using `%%sql`, with the outputs rendered as tables directly in the notebook. This made it easy to iterate and annotate the results with short explanations.

---

## Project 2 – Human Resources Database

Folder: [`Human Resources Database`](Human%20Resources%20Database)

SQL scripts:

- **DDL (schema design)**  
  - [`HR database-DDL.sql`](Human%20Resources%20Database/HR%20database-DDL.sql)
- **Queries – Part 1**  
  - [`HR database-Queries.sql`](Human%20Resources%20Database/HR%20database-Queries.sql)
- **Queries – Part 2 (joins and more)**  
  - [`HR database-Queries_2.sql`](Human%20Resources%20Database/HR%20database-Queries_2.sql)

### Backstory

This project is a mini **HR system** for a fictional company.  
The idea was to:

- Design a simple HR schema from scratch, and  
- Practice writing realistic queries that an HR analyst or HRIS system might run.

Instead of using a large real dataset, everything here is **schema + queries**: focusing on table design, relationships, joins, and aggregations.

### Schema design

The `HR database-DDL.sql` script builds the core tables:

- `EMPLOYEES` – employee id, first/last name, SSN, birth date, sex, address, job id, salary, manager id, department id  
- `JOBS` – job identifier, job title, minimum and maximum salary  
- `JOB_HISTORY` – records of previous job assignments for each employee  
- `DEPARTMENTS` – department id, name, manager, and location id  
- `LOCATIONS` – locations tied to departments  

This schema models:

- **One-to-many** relationships (department → employees, job → employees)  
- **Historical records** for people who have changed jobs (`JOB_HISTORY`)  
- **Geographical / organisational hierarchy** via locations and departments  

### Query examples and what they show

The two query scripts show how to ask common HR questions using SQL:

From [`HR database-Queries.sql`](Human%20Resources%20Database/HR%20database-Queries.sql):

- Filter employees by **address**  
  - e.g. employees whose address contains `Elgin,IL`.  
- Filter by **birth decade**  
  - employees born in the 1970s using `B_DATE` patterns.  
- Find employees in a **salary band** and **department**  
  - e.g. salary between 60,000 and 70,000 in department 5.  
- Group by **department** to compute:
  - `COUNT(*)` (headcount)  
  - `AVG(SALARY)` (average salary)  
  and then use `HAVING` to keep only departments with fewer than 4 employees.

From [`HR database-Queries_2.sql`](Human%20Resources%20Database/HR%20database-Queries_2.sql):

- Use **INNER JOIN** between `EMPLOYEES` and `JOB_HISTORY` to list:
  - Employees in department 5 and their job start dates.  
- Add a join to `JOBS` to show **job titles** along with employee names and start dates.  
- Compare **LEFT OUTER JOIN** vs **INNER JOIN** to see how unmatched rows behave:
  - For employees born before 1980 and their departments.  
- Use **FULL OUTER JOIN / LEFT JOIN** between `EMPLOYEES` and `DEPARTMENTS`:
  - To list employees with department names,  
  - Or to filter based on criteria like gender (`SEX='M'`).

### What this project demonstrates

This HR database project shows:

- How to turn a **business domain** (HR) into a relational schema.  
- How to use **DDL** to define tables and primary keys.  
- How to write **DML / query scripts** that:
  - Filter rows using `WHERE`, `BETWEEN`, `LIKE`, and functions like `YEAR()`.  
  - Combine tables using `INNER`, `LEFT`, and `FULL OUTER` joins.  
  - Aggregate and summarise data using `GROUP BY` and `HAVING`.  

It’s a good example of using pure SQL to answer practical questions about employees, departments, and compensation, and it can easily be run either in a Db2 console or from Jupyter using the same `%load_ext sql` pattern as the Chicago notebook.

---

## Project 3 – Pet Sale Database

Folder: [`Pet Sale Database`](Pet%20Sale%20Database)

SQL scripts:

- [`DDL - Exercise.sql`](Pet%20Sale%20Database/DDL%20-%20Exercise.sql)  
- [`PETSALE-CREATE.sql`](Pet%20Sale%20Database/PETSALE-CREATE.sql)  
- [`PETSALE-FUNCTIONS.sql`](Pet%20Sale%20Database/PETSALE-FUNCTIONS.sql)

### Backstory

This one is a lightweight, sandbox-style project.  
The idea: build a tiny **pet shop** sales dataset and then use it to practice SQL functions and basic DDL/DML without the complexity of a big schema.

It’s intentionally small and simple so the focus stays on the SQL itself.

### Schema and sample data

In [`PETSALE-CREATE.sql`](Pet%20Sale%20Database/PETSALE-CREATE.sql):

- Drop the `PETSALE` table if it exists.  
- Create a new `PETSALE` table with:

  ```sql
  create table PETSALE (
      ID        INTEGER PRIMARY KEY NOT NULL,
      ANIMAL    VARCHAR(20),
      QUANTITY  INTEGER,
      SALEPRICE DECIMAL(6,2),
      SALEDATE  DATE
  );
  ```

- Insert a handful of rows, for example:
  - Cats, dogs, parrots, hamsters, goldfish  
  - Various quantities and sale prices  
  - Sales across late May and June 2018  

In [`DDL - Exercise.sql`](Pet%20Sale%20Database/DDL%20-%20Exercise.sql) there is also an extended exercise that:

- Creates a `PETSALE` table with both `SALEPRICE` and `PROFIT`, and  
- A `PET` table with `ANIMAL` and `QUANTITY`,  
- Then experiments with inserts, updates, and joins as part of the practice.

### Practising SQL functions

[`PETSALE-FUNCTIONS.sql`](Pet%20Sale%20Database/PETSALE-FUNCTIONS.sql) runs a bunch of built‑in SQL functions against the `PETSALE` data:

Numeric and aggregate functions:

- `SUM(SALEPRICE)` – total revenue  
- `MAX(QUANTITY)` – largest quantity sold in one row  
- `AVG(SALEPRICE)` – average sale price  
- `AVG(SALEPRICE / QUANTITY)` for dogs – average per‑unit price  

String functions:

- `LENGTH(ANIMAL)` – length of each animal name  
- `UCASE(ANIMAL)` / `LCASE(ANIMAL)` – upper/lowercase conversions  
- `DISTINCT(UCASE(ANIMAL))` – unique animal types in uppercase  
- Filter rows with case-insensitive conditions, e.g.:

  ```sql
  select * from PETSALE where LCASE(ANIMAL) = 'cat';
  ```

Date functions:

- `DAY(SALEDATE)` – day of the month (for a given animal)  
- `COUNT(*)` where `MONTH(SALEDATE)='05'` – sales in May  
- `(SALEDATE + 3 DAYS)` – add three days to the sale date  
- `(CURRENT DATE - SALEDATE)` – days since each sale  

### What this project demonstrates

This pet shop mini-database shows:

- How to **create and populate** a small table quickly.  
- How to use **aggregate functions** for basic reporting.  
- How to work with **string and date functions** in SQL.  
- How to test and understand expressions like date arithmetic and case‑insensitive matching.

It’s a nice “lab bench” for learning SQL without getting distracted by a complicated schema.

---

## Overall Skills Demonstrated in this Repo

Across these three projects, I worked with:

- **DDL** (CREATE TABLE, primary keys, basic design)  
- **DML and queries** (SELECT, INSERT, UPDATE, DELETE in the exercises)  
- **Joins** across multiple related tables  
- **Aggregations and groupings** for reporting  
- **Built‑in functions** for numbers, strings, and dates  
- **Database connectivity from Jupyter**, using the `sql` magic extension and Db2 drivers  

The Chicago notebook shows how to wire up a real cloud database into a Python notebook, while the HR and Pet Sale projects show how to design schemas and write queries that match real‑world questions.

---

## Author

**Yasir Savanur**  

- LinkedIn: https://www.linkedin.com/in/yasir-savanur/

These exercises are based on the “Databases and SQL for Data Science” course material, extended with my own notes and practice queries.
