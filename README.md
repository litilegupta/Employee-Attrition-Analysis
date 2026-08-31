📊 HR Analytics & Employee Attrition | SQL & Power BI

A complete HR Analytics project using SQL Server for data cleaning and analysis and Power BI for interactive dashboard creation. The project focuses on understanding employee attrition and identifying workforce patterns.

📌 Project Overview

This project analyses employee HR data to understand employee attrition, job roles, departments, salary, experience, demographics, and work conditions.

SQL was used to clean and analyse the data, while Power BI was used to create an interactive dashboard for business insights.

🎯 Objectives

Clean and prepare HR data using SQL.

Identify and remove duplicate records.

Standardise inconsistent data values.

Analyse employee attrition using SQL.

Create SQL views for Power BI.

Build an interactive Power BI dashboard.

Generate meaningful HR insights.

🛠️ Technologies Used

SQL Server

SQL

Power BI

Power Query

Data Cleaning

Data Visualization

📂 Dataset

The dataset contains 1,470 employee records and 41 columns.

Important fields include:

Employee ID

Age

Gender

Department

Job Role

Job Level

Education Field

Monthly Income

Years At Company

Over Time

Job Satisfaction

Work Life Balance

Business Travel

Attrition

Attrition

Yes → Employee left the company
No  → Employee stayed

A numerical attrition_value column was created:

Yes → 1
No  → 0

This was used to calculate attrition counts and percentages.

🧹 SQL Data Cleaning

1. Create Attrition Value

ALTER TABLE dbo.HR_Analytics
ADD attrition_value INT;

UPDATE dbo.HR_Analytics
SET attrition_value =
CASE
    WHEN attrition = 'Yes' THEN 1
    ELSE 0
END;

This converts the Attrition field into numerical values for easier analysis.

2. Remove Unused Column

The YearsWithCurrManager column was removed as it was not useful for the planned analysis.

ALTER TABLE dbo.HR_Analytics
DROP COLUMN YearsWithCurrManager;

3. Check and Remove Duplicates

Duplicate employee records were identified using ROW_NUMBER() and PARTITION BY.

Distinct records were then stored temporarily in HR2, the original table was truncated, and the cleaned records were inserted back.

SELECT DISTINCT *
INTO dbo.HR2
FROM dbo.HR_Analytics;

After reinserting the distinct records, the temporary table was deleted.

4. Standardise Business Travel

Inconsistent values such as TravelRarely and Travel_Rarely were identified and standardised.

UPDATE dbo.HR_Analytics
SET BusinessTravel = 'Travel_Rarely'
WHERE BusinessTravel = 'TravelRarely';

📈 SQL Data Analysis

After cleaning, SQL was used to analyse employee attrition across different dimensions:

Attrition by Age Group

Attrition by Department

Attrition by Education Field

Attrition by Job Role

Attrition by Salary Slab

Attrition by Years at Company

Attrition by Overtime

Attrition by Gender

Attrition percentage was calculated using:

Attrition Rate =
Employees Attrited / Total Employees × 100

SQL views were created for the analysis:

AttVSAge
AttVSDep
AttVSEdu
AttVSJob
AttVSSal
AttVSYrs
AttVSOverT
AttVSGen

A CTE (Common Table Expression) was also used for age-group attrition analysis.

📊 Power BI Dashboard

The cleaned SQL data and analysis were used to build an interactive Power BI dashboard.

Dashboard Features

Total Employees

Attrition Count

Attrition Percentage

Active Employees

Attrition by Age Group

Attrition by Department

Attrition by Education Field

Attrition by Job Role

Attrition by Salary Slab

Attrition by Years at Company

Attrition by Overtime

Attrition by Gender

Interactive Filters / Slicers

💡 Key Insights

Analysed employee attrition across different age groups and departments.

Identified variations in attrition across job roles and salary levels.

Examined the relationship between employee tenure and attrition.

Compared attrition between employees working overtime and those who do not.

Analysed attrition patterns across gender and education fields.

Created an interactive dashboard to support HR decision-making.

📁 Project Structure

HR-Analytics-Employee-Attrition/
│
├── Dataset/
│   └── HR Data (1).csv
│
├── SQL/
│   └── HR_Analytics.sql
│
├── PowerBI/
│   └── HR_Analytics_Dashboard.pbix
│
├── Dashboard/
│   └── Dashboard.png
│
└── README.md

🚀 Future Improvements

Add advanced DAX measures.

Analyse employee satisfaction and promotion patterns.

Add employee retention recommendations.

Add predictive attrition analysis.

Automate SQL and Power BI data refresh.

⭐ Conclusion

This project demonstrates an end-to-end HR Analytics solution using SQL Server and Power BI. SQL was used for data cleaning and analysis, while Power BI was used to create an interactive dashboard for understanding employee attrition and workforce patterns.

👨‍💻 Author

Litile Gupta

⭐ If you found this project useful, consider giving it a Star!
