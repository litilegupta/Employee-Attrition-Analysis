# 📊 HR Analytics & Employee Attrition | SQL & Power BI

An end-to-end **HR Analytics project** using **SQL Server** for data cleaning and analysis and **Power BI** for creating an interactive dashboard. The project analyses employee attrition and workforce patterns across departments, job roles, salary, age, education, experience, overtime, and gender.

---

## 📌 Project Overview

Employee attrition is an important HR business problem because high employee turnover can increase recruitment and training costs and affect workforce stability.

In this project, I analysed an HR dataset containing **1,470 employee records**. I first cleaned and prepared the data using SQL Server, performed exploratory analysis, created SQL views, and then used Power BI to build an interactive HR Analytics dashboard.

### Project Workflow

```text
Raw HR Dataset
      ↓
Data Import into SQL Server
      ↓
Data Cleaning
      ↓
Duplicate & Data Quality Checks
      ↓
SQL Exploratory Analysis
      ↓
SQL Views
      ↓
Power BI
      ↓
Interactive HR Analytics Dashboard
```

---

## 🎯 Business Objectives

- Understand the overall employee workforce.
- Analyse employee attrition patterns.
- Identify departments and job roles with different attrition levels.
- Analyse attrition across age groups, education fields, salary slabs, and tenure.
- Compare attrition between employees who work overtime and those who do not.
- Analyse workforce distribution by gender.
- Present the findings through an interactive Power BI dashboard.

---

## 🛠️ Tools & Technologies

- **SQL Server** – Data cleaning and analysis
- **SQL** – Data transformation, aggregation and analytical queries
- **Power BI** – Dashboard and visualization
- **Power Query** – Data preparation
- **CSV** – Source dataset

### SQL Concepts Used

- `SELECT`
- `ALTER TABLE`
- `UPDATE`
- `CASE WHEN`
- `GROUP BY`
- `COUNT()`
- `SUM()`
- `AVG()`
- `CONVERT()`
- `SELECT DISTINCT`
- `ROW_NUMBER()`
- `PARTITION BY`
- `CREATE VIEW`
- `CTE`

---

# 📂 Dataset

The HR dataset contains **1,470 employee records** with information related to employee demographics, job details, compensation, experience, satisfaction, work conditions, and attrition.

### Important Fields

| Category | Fields |
|---|---|
| Employee | Employee ID, Age, Gender, Marital Status |
| Education | Education, Education Field |
| Job | Department, Job Role, Job Level |
| Salary | Monthly Income, Salary Slab, Percent Salary Hike |
| Experience | Total Working Years, Years At Company, Years In Current Role |
| Work Conditions | Over Time, Business Travel, Distance From Home |
| Satisfaction | Job Satisfaction, Environment Satisfaction, Work Life Balance |
| Performance | Performance Rating, Job Involvement |
| Target | Attrition |

### Attrition

The `Attrition` column contains:

```text
Yes → Employee left the company
No  → Employee stayed in the company
```

To make calculations easier, a numerical column called `attrition_value` was created:

```text
Yes → 1
No  → 0
```

This makes it possible to calculate the number of employees who left using:

```sql
SUM(attrition_value)
```

---

# 🧹 SQL Data Cleaning

## 1. Creating Attrition Value

The first step was to convert the categorical Attrition field into a numerical field.

```sql
ALTER TABLE dbo.HR_Analytics
ADD attrition_value INT;

UPDATE dbo.HR_Analytics
SET attrition_value =
CASE
    WHEN attrition = 'Yes' THEN 1
    ELSE 0
END;
```

This was done because numerical values make aggregation and attrition-rate calculations easier.

For example:

```text
Attrition = Yes → 1
Attrition = No  → 0
```

Therefore:

```sql
SUM(attrition_value)
```

returns the number of employees who left.

---

## 2. Removing an Unused Column

During data inspection, `YearsWithCurrManager` was identified as a column that was not useful for the planned analysis and contained unusable/blank information.

It was removed using:

```sql
ALTER TABLE dbo.HR_Analytics
DROP COLUMN YearsWithCurrManager;
```

---

## 3. Checking for Duplicate Records

Duplicate employee records were checked using `ROW_NUMBER()` and `PARTITION BY`.

```sql
SELECT
    empid,
    ROW_NUMBER() OVER (
        PARTITION BY empid
        ORDER BY empid
    ) AS num_rows
FROM dbo.HR_Analytics
ORDER BY 2 DESC;
```

### Why `ROW_NUMBER()`?

`PARTITION BY empid` creates a separate group for each employee ID.

If an employee appears more than once, the employee will have multiple row numbers:

```text
empid    num_rows
101      1
101      2
```

This makes duplicate records easy to identify.

---

## 4. Removing Duplicate Records

After identifying duplicate records, distinct records were copied into a temporary table:

```sql
SELECT DISTINCT *
INTO dbo.HR2
FROM dbo.HR_Analytics;
```

The original table was then cleared:

```sql
TRUNCATE TABLE dbo.HR_Analytics;
```

The cleaned records were inserted back:

```sql
INSERT INTO dbo.HR_Analytics
SELECT *
FROM dbo.HR2;
```

Finally, the temporary table was deleted:

```sql
DROP TABLE dbo.HR2;
```

The duplicate check was performed again to verify the cleaning process.

---

## 5. Standardising Business Travel Values

During data quality checking, inconsistent values were found in the `BusinessTravel` column.

For example:

```text
Travel_Rarely
TravelRarely
```

These represent the same category but would be treated as different values during analysis.

The inconsistent value was corrected:

```sql
UPDATE dbo.HR_Analytics
SET BusinessTravel = 'Travel_Rarely'
WHERE BusinessTravel = 'TravelRarely';
```

The values were then checked again to confirm that the categories had been standardised.

---

# 📈 SQL Exploratory Analysis

After completing the cleaning process, SQL was used to explore employee attrition and workforce patterns.

The analysis focused on:

- Age Group
- Department
- Education Field
- Job Role
- Salary Slab
- Years At Company
- Overtime
- Gender

The general attrition calculation used in the analysis was:

```text
Attrition Rate =
Number of Employees Attrited
---------------------------- × 100
Total Employees
```

The numerical `attrition_value` column made this calculation easier.

---

# 👁️ SQL Views

To make the analysis reusable, SQL views were created for different HR dimensions.

### Views Created

```text
AttVSAge
AttVSDep
AttVSEdu
AttVSJob
AttVSSal
AttVSYrs
AttVSOverT
AttVSGen
```

Each view contains grouped employee information along with:

- Total employees
- Number of employees who left
- Attrition percentage

For example, the department view can be accessed using:

```sql
SELECT *
FROM AttVSDep;
```

This makes the processed results easier to use in Power BI.

---

# 🔎 CTE Analysis

A Common Table Expression (CTE) was also used for age-group analysis.

```sql
WITH cte (AgeGroup, emp_attri, count_age) AS
(
    SELECT
        AgeGroup,
        SUM(Attrition_value),
        COUNT(AgeGroup)
    FROM dbo.HR_Analytics
    GROUP BY AgeGroup
)
SELECT *,
    (
        CONVERT(FLOAT, emp_attri)
        /
        CONVERT(FLOAT, count_age)
    ) * 100 AS Attrition_Percentage
FROM cte
ORDER BY emp_attri DESC;
```

This demonstrates the use of a CTE to temporarily store an aggregated result and perform further calculations.

---

# 📊 Power BI Dashboard

After the SQL cleaning and analysis, the data was used to create an interactive Power BI dashboard.

## Dashboard KPIs

The top section of the dashboard contains:

- **Overall Employees**
- **Attrition**
- **Attrition Rate**
- **Active Employees**
- **Average Age**

These KPIs provide a quick overview of the organisation's workforce.

---

## Dashboard Visuals

### 1. Department-wise Attrition

Shows the distribution of employee attrition across:

- R&D
- Sales
- HR

This helps compare the contribution of different departments to total attrition.

### 2. Number of Employees by Age Group

Displays employee distribution across different age groups and provides a gender-wise breakdown.

### 3. Job Satisfaction Rating

Shows job satisfaction ratings across different job roles, helping compare employee satisfaction between roles.

### 4. Education Field-wise Attrition

Displays attrition across different education fields such as Life Sciences, Medical, Marketing, Technical Degree, and others.

### 5. Attrition Rate by Gender for Different Age Groups

Provides a detailed comparison of attrition across gender and age-group combinations.

---

# 🎛️ Dashboard Interactivity

The Power BI dashboard includes interactive filters/slicers that allow users to analyse the data based on education level and other available dimensions.

Users can interact with the dashboard to explore employee attrition from different perspectives rather than looking at only the overall numbers.

---

# 💡 Key Insights

The project helps identify:

- Workforce distribution across departments and job roles.
- Attrition differences across age groups.
- Attrition patterns across education fields.
- Differences in attrition across salary slabs.
- How employee tenure relates to attrition.
- Differences in attrition between employees who work overtime and those who do not.
- Gender-wise differences in employee attrition.
- Job satisfaction patterns across different job roles.

These insights can help HR teams identify employee groups that may require further investigation for retention strategies.

> **Note:** The analysis identifies patterns and associations in the dataset. It does not prove that a particular factor directly causes employee attrition.

---

# 📁 Project Structure

```text
HR-Analytics-Employee-Attrition/
│
├── HR Data.csv
│
├── HR_Analytics.sql
│
├── HR_Analytics_Dashboard.pbix
│
└── README.md
```

---

# 🚀 How to Run the Project

### Step 1 — Import the Dataset

Import the HR CSV file into SQL Server and create the table:

```text
dbo.HR_Analytics
```

### Step 2 — Run the SQL Script

Execute the SQL script in order:

```text
1. Create attrition_value
2. Remove unused column
3. Check duplicate records
4. Remove duplicate records
5. Standardise Business Travel values
6. Perform exploratory analysis
7. Create SQL views
```

### Step 3 — Open Power BI

Open the `.pbix` dashboard file in Power BI Desktop.

### Step 4 — Refresh the Data

Refresh the data source/model after connecting to the cleaned SQL data.

### Step 5 — Explore the Dashboard

Use the available filters and visuals to analyse employee attrition and workforce patterns.

---

# 🎓 Skills Demonstrated

### SQL

- Data Cleaning
- Data Quality Checks
- Duplicate Detection
- Data Transformation
- Aggregate Functions
- Conditional Logic
- Window Functions
- CTEs
- SQL Views
- Attrition Rate Calculation

### Power BI

- Dashboard Design
- KPI Cards
- Data Visualization
- Interactive Filters
- HR Reporting

### Business Analytics

- Employee Attrition Analysis
- Workforce Analysis
- Exploratory Data Analysis
- Employee Retention Analysis

---

# 🚀 Future Improvements

- Add deeper analysis of employee satisfaction.
- Analyse promotion and career progression patterns.
- Add HR recommendations based on key findings.
- Add advanced DAX measures.
- Add predictive employee attrition analysis.
- Automate SQL Server to Power BI data refresh.

---

# ⭐ Conclusion

This project demonstrates an end-to-end **HR Analytics workflow using SQL Server and Power BI**.

The project starts with raw employee data, performs data cleaning and quality checks in SQL, analyses employee attrition using SQL queries and views, and finally presents the findings through an interactive Power BI dashboard.

The dashboard provides a business-friendly way to understand **employee attrition and workforce patterns** and can support further HR investigation and retention planning.

---

## 👨‍💻 Author

**Litile Gupta**
