# 📊 HR Analytics & Employee Attrition | SQL & Power BI

A data analytics project using **SQL Server** and **Power BI** to analyse employee attrition and workforce patterns.

## 📌 Project Overview

The project covers data cleaning, SQL analysis, and an interactive Power BI dashboard to understand employee attrition across different HR factors.

## 🎯 Objectives

- Clean HR data using SQL
- Identify and remove duplicate records
- Standardise inconsistent values
- Analyse employee attrition
- Create SQL views for analysis
- Build an interactive Power BI dashboard

## 🛠️ Tools Used

- SQL Server
- SQL
- Power BI
- Power Query

## 📂 Dataset

The dataset contains **1,470 employee records** with information about:

- Employee demographics
- Department and job role
- Salary and experience
- Job satisfaction
- Overtime and business travel
- Employee attrition

`Attrition = Yes` means the employee left the company, while `No` means the employee stayed.

## 🧹 SQL Data Cleaning

### 1. Create Attrition Value

Converted `Attrition` into numerical values:

```sql
CASE
    WHEN Attrition = 'Yes' THEN 1
    ELSE 0
END
```

### 2. Remove Duplicate Records

Used `ROW_NUMBER()` and `PARTITION BY` to identify duplicate employee records and retained distinct records.

### 3. Standardise Data

Corrected inconsistent `BusinessTravel` values such as `TravelRarely` and `Travel_Rarely`.

## 📈 SQL Analysis

Analysed attrition by:

- Age Group
- Department
- Education Field
- Job Role
- Salary Slab
- Years at Company
- Overtime
- Gender

Created SQL views for these analyses and used a CTE for additional analysis.

## 📊 Power BI Dashboard

The dashboard includes:

- Total Employees
- Attrition
- Attrition Rate
- Active Employees
- Average Age
- Department-wise Attrition
- Employees by Age Group
- Job Satisfaction
- Education-wise Attrition
- Gender and Age Group Attrition

## 💡 Key Insights

- Identified employee attrition patterns across different groups.
- Compared attrition across departments, job roles, salary levels, and tenure.
- Analysed the effect of overtime and employee demographics on attrition.
- Built an interactive dashboard to support HR decision-making.

## 📁 Project Structure

```text
HR-Analytics-Employee-Attrition/
│
├── Dataset/
├── SQL/
├── PowerBI/
├── Dashboard/
└── README.md
```

## 🚀 Future Improvements

- Add advanced DAX measures
- Add employee retention recommendations
- Add predictive attrition analysis
- Automate data refresh

## ⭐ Conclusion

This project demonstrates an end-to-end **HR Analytics workflow using SQL Server and Power BI**, from data cleaning and analysis to interactive dashboard creation.

## 👨‍💻 Author

**Litile Gupta**
