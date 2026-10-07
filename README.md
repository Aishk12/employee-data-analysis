# Employee Data Analysis — Oracle SQL & Python

## 📌 Project Overview

This project analyzes employee and manager data stored in an Oracle database. The data is extracted from Oracle using Python and SQLAlchemy, analyzed using SQL and Pandas, and visualized using Python and Power BI.

The project demonstrates an end-to-end data analysis workflow, starting from data storage and extraction to data cleaning, exploratory analysis, visualization, and business insights.

---

## 🎯 Project Objectives

The main objectives of this project are to:

* Analyze employee data stored in an Oracle database
* Perform SQL-based data analysis
* Connect Oracle Database with Python
* Load and analyze data using Pandas
* Perform data cleaning and exploratory data analysis
* Analyze employee salary and department trends
* Understand manager-employee relationships
* Create meaningful data visualizations
* Build a Power BI dashboard
* Generate business insights from the data

---

## 🛠️ Tools & Technologies

* **Oracle Database XE** — Data storage
* **Oracle SQL Developer** — Database management and SQL queries
* **SQL** — Data extraction and analysis
* **Python** — Data analysis
* **Pandas** — Data manipulation and analysis
* **SQLAlchemy** — Database connectivity
* **oracledb** — Oracle database connection
* **Jupyter Notebook** — Analysis environment
* **Matplotlib** — Data visualization
* **Power BI** — Interactive dashboard
* **GitHub** — Project version control and portfolio

---

## 🔄 Project Workflow

```text
Excel Data
     ↓
Oracle Database
     ↓
SQL Queries
     ↓
Python + SQLAlchemy
     ↓
Pandas DataFrames
     ↓
Data Cleaning
     ↓
Exploratory Data Analysis
     ↓
Data Visualization
     ↓
Power BI Dashboard
     ↓
Business Insights
```

---

## 🗄️ Database Structure

The project uses two main tables:

### Employees

The `EMPLOYEES` table contains employee-level information such as:

* Employee ID
* Employee Name
* Department
* Job Title
* Salary
* Manager ID
* Hire Date
* Location

### Managers

The `MANAGERS` table contains manager information such as:

* Manager ID
* Manager Name
* Department
* Job Title
* Salary
* Hire Date
* Location

The two tables are connected using `MANAGER_ID`.

---

## 🔗 Database Relationship

```text
MANAGERS
---------
manager_id (Primary Key)
manager_name
department
job_title
salary
hire_date
location
       │
       │
       │  manager_id
       ↓
EMPLOYEES
---------
employee_id (Primary Key)
employee_name
department
job_title
salary
manager_id (Foreign Key)
hire_date
location
```

---

## 🐍 Connecting Oracle with Python

Python is connected to Oracle using SQLAlchemy and the Oracle database driver.

Example:

```python
import pandas as pd
from sqlalchemy import create_engine

username = "YOUR_USERNAME"
password = "YOUR_PASSWORD"

host = "localhost"
port = 1521
service_name = "XE"

connection_string = (
    f"oracle+oracledb://{username}:{password}"
    f"@{host}:{port}/?service_name={service_name}"
)

engine = create_engine(connection_string)
```

> **Security Note:** Actual database credentials are not included in this repository.

---

## 📥 Loading Data into Pandas

Employee and manager data are extracted from Oracle and loaded into Pandas DataFrames.

```python
employees_df = pd.read_sql(
    "SELECT * FROM SYSTEM.EMPLOYEES",
    engine
)

managers_df = pd.read_sql(
    "SELECT * FROM SYSTEM.MANAGERS",
    engine
)
```

The employee dataset contains **100 employee records**.

---

## 🔍 Data Exploration

The dataset is explored using Pandas to understand its structure and quality.

### Dataset dimensions

```python
employees_df.shape
```

### Dataset information

```python
employees_df.info()
```

### Statistical summary

```python
employees_df.describe()
```

### Missing values

```python
employees_df.isnull().sum()
```

### Duplicate records

```python
employees_df.duplicated().sum()
```

---

## 🧹 Data Cleaning

The following data quality checks are performed:

* Missing value detection
* Duplicate record detection
* Data type validation
* Text formatting and whitespace cleanup
* Salary validation
* Employee ID uniqueness checks
* Manager relationship validation

Example:

```python
employees_df["EMPLOYEE_NAME"] = (
    employees_df["EMPLOYEE_NAME"].str.strip()
)

employees_df["DEPARTMENT"] = (
    employees_df["DEPARTMENT"].str.strip()
)
```

---

## 🔎 SQL Analysis

SQL is used to answer business questions from the Oracle database.

### Total Employees

```sql
SELECT COUNT(*) AS total_employees
FROM SYSTEM.EMPLOYEES;
```

### Employees by Department

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM SYSTEM.EMPLOYEES
GROUP BY department
ORDER BY employee_count DESC;
```

### Average Salary by Department

```sql
SELECT
    department,
    ROUND(AVG(salary), 2) AS average_salary
FROM SYSTEM.EMPLOYEES
GROUP BY department
ORDER BY average_salary DESC;
```

### Highest-Paid Employees

```sql
SELECT
    employee_id,
    employee_name,
    department,
    job_title,
    salary
FROM SYSTEM.EMPLOYEES
ORDER BY salary DESC
FETCH FIRST 10 ROWS ONLY;
```

### Employees by Location

```sql
SELECT
    location,
    COUNT(*) AS employee_count
FROM SYSTEM.EMPLOYEES
GROUP BY location
ORDER BY employee_count DESC;
```

---

## 🔗 Employee & Manager Analysis

Employee and manager information is combined using a SQL `LEFT JOIN`.

```sql
SELECT
    e.employee_id,
    e.employee_name,
    e.department,
    e.job_title,
    e.salary,
    e.manager_id,
    e.hire_date,
    e.location,
    m.manager_name
FROM SYSTEM.EMPLOYEES e
LEFT JOIN SYSTEM.MANAGERS m
    ON e.manager_id = m.manager_id;
```

This allows analysis of employee-manager relationships and reporting structures.

---

## 🐼 Python & Pandas Analysis

Pandas is used to perform additional analysis.

### Employees by Department

```python
department_count = (
    employees_df
    .groupby("DEPARTMENT")
    .size()
    .sort_values(ascending=False)
)

display(department_count)
```

### Average Salary by Department

```python
avg_salary = (
    employees_df
    .groupby("DEPARTMENT")["SALARY"]
    .mean()
    .sort_values(ascending=False)
)

display(avg_salary)
```

### Top 10 Highest-Paid Employees

```python
top_salary = employees_df.nlargest(10, "SALARY")

display(
    top_salary[
        ["EMPLOYEE_NAME", "DEPARTMENT", "JOB_TITLE", "SALARY"]
    ]
)
```

### Employees by Location

```python
location_count = employees_df["LOCATION"].value_counts()

display(location_count)
```

---

## 📊 Data Visualization

Python visualizations are used to identify patterns and trends in the employee data.

The project includes visualizations such as:

* Employee distribution by department
* Employee distribution by location
* Average salary by department
* Salary distribution
* Top-paid employees
* Employee hiring trends

Example:

```python
import matplotlib.pyplot as plt

department_count.plot(kind="bar")

plt.title("Employees by Department")
plt.xlabel("Department")
plt.ylabel("Number of Employees")
plt.xticks(rotation=45)
plt.show()
```

---

## 📈 Power BI Dashboard

A Power BI dashboard is created to provide an interactive view of the employee data.

### Dashboard includes:

* Total Employees
* Average Salary
* Employee Count by Department
* Employee Count by Location
* Average Salary by Department
* Salary Distribution
* Top-Paid Employees
* Manager/Employee analysis

Dashboard screenshots are available in the `images` folder.

---

## 💡 Business Questions

The analysis focuses on questions such as:

1. How many employees are in the organization?
2. Which department has the highest number of employees?
3. Which department has the highest average salary?
4. Which locations have the highest employee concentration?
5. Who are the highest-paid employees?
6. How are employees distributed among managers?
7. Are there missing or duplicate records?
8. What salary patterns can be observed?
9. What hiring trends can be identified?

---

## 📌 Key Insights

The analysis is used to identify:

* Workforce distribution across departments
* Salary differences between departments
* Employee concentration across locations
* Highest-paid employees
* Manager-employee reporting relationships
* Data quality issues
* Workforce and hiring trends

> Specific insights and values are documented in the Jupyter Notebook and Power BI dashboard based on the analyzed dataset.

---

## 📁 Project Structure

```text
employee-data-analysis/
│
├── README.md
│
├── data/
│   └── employee_data.xlsx
│
├── sql/
│   └── employee_analysis.sql
│
├── notebooks/
│   └── employee_analysis.ipynb
│
├── powerbi/
│   └── employee_analysis_dashboard.pbix
│
├── images/
│   ├── dashboard.png
│   ├── employees_analysis.png
│   └── salary_analysis.png
│
└── requirements.txt
```

---

## 📦 Python Requirements

The main Python libraries used in this project are:

```text
pandas
sqlalchemy
oracledb
matplotlib
jupyter
```

They can be installed using:

```bash
pip install pandas sqlalchemy oracledb matplotlib jupyter
```

---

## 🚀 How to Run the Project

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/employee-data-analysis.git
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Set up Oracle Database

Create the required `EMPLOYEES` and `MANAGERS` tables and load the dataset into Oracle.

### 4. Configure the database connection

Update the Oracle connection details in the notebook.

**Do not commit real passwords or credentials to GitHub.**

### 5. Open the Jupyter Notebook

```bash
jupyter notebook
```

Open:

```text
notebooks/employee_analysis.ipynb
```

### 6. Run the analysis

Execute the notebook cells sequentially to:

* Connect to Oracle
* Extract data
* Clean the data
* Perform analysis
* Create visualizations
* Generate insights

---

## 🎓 Skills Demonstrated

This project demonstrates practical experience with:

* SQL querying
* Oracle Database
* Relational data
* SQL joins
* Aggregation
* Data extraction
* Python
* Pandas
* Data cleaning
* Exploratory Data Analysis
* Data visualization
* Business analysis
* Power BI
* Database connectivity
* Data quality validation

---

## 📌 Conclusion

This project demonstrates an end-to-end approach to analyzing structured employee data using Oracle SQL, Python, Pandas, and Power BI.

It combines database querying, data preparation, exploratory analysis, visualization, and business interpretation to transform raw employee data into meaningful insights.

---

## 👩‍💻 Author

**Aishwarya Kamble**

Data Analyst | Data Analytics Enthusiast

GitHub:
https://github.com/Aishk12

LinkedIn:
https://www.linkedin.com/in/aishkamble5/
