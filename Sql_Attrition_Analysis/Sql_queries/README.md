# 🗄️ SQL Analysis — IBM HR Employee Attrition Analytics

## Overview

PostgreSQL was used to analyze the **IBM HR Analytics Employee Attrition & Performance** dataset and answer the HR Director's business questions through structured SQL queries.

The SQL analysis forms the analytical bridge between the cleaned dataset and the final Power BI dashboard.

The objective was not simply to query the data, but to translate business questions about **employee turnover, compensation, employee experience, and workforce risk** into measurable SQL analysis.

---

# 🎯 Business Objective

Employee turnover is increasing, and management wants to understand why employees leave and which workforce groups experience the highest attrition.

The SQL analysis investigates:

* Overall employee attrition
* Department-level attrition
* Job role attrition
* Overtime and attrition
* Compensation and retention
* Age and attrition
* Work-life balance
* Employee satisfaction
* Career progression
* Employee risk segmentation

---

# 🛠️ Technology

* **Database:** PostgreSQL
* **Interface:** pgAdmin 4
* **Language:** SQL
* **Source Data:** IBM HR Analytics Employee Attrition & Performance

---

# 📂 Dataset

The dataset contains **1,470 employee records** and **35 variables** covering:

* Demographics
* Employment information
* Department
* Job role
* Compensation
* Overtime
* Job satisfaction
* Environment satisfaction
* Work-life balance
* Performance
* Career progression
* Employee tenure

### Primary Table

```text
employee_attrition
```

### Key Fields

```text
EmployeeNumber
Age
Attrition
BusinessTravel
Department
DistanceFromHome
Education
EducationField
Gender
JobLevel
JobRole
JobSatisfaction
MaritalStatus
MonthlyIncome
MonthlyRate
NumCompaniesWorked
OverTime
PercentSalaryHike
PerformanceRating
RelationshipSatisfaction
StockOptionLevel
TotalWorkingYears
TrainingTimesLastYear
WorkLifeBalance
YearsAtCompany
YearsInCurrentRole
YearsSinceLastPromotion
YearsWithCurrManager
```

---

# 🔄 SQL Analysis Workflow

```text
Cleaned Excel Dataset
        ↓
CSV Export
        ↓
PostgreSQL Database
        ↓
Table Creation
        ↓
Data Import
        ↓
Data Quality Validation
        ↓
Exploratory SQL Analysis
        ↓
Stakeholder Business Questions
        ↓
Risk Analysis
        ↓
Insights
        ↓
Power BI Validation
```

---
# 01 — Table Creation

### File

`01_create_table.sql`

This script creates the PostgreSQL table used for the analysis.

The table was designed around the structure of the cleaned IBM HR dataset.

### Purpose

* Define column names
* Define appropriate data types
* Establish the employee identifier
* Prepare the database for CSV import
* Create a consistent SQL analysis environment

### Primary Key

```sql
EmployeeNumber
```

`EmployeeNumber` uniquely identifies each employee in the dataset.

---

# 02 — Data Quality Checks

### File

`02_data_quality_checks.sql`

Before performing business analysis, SQL queries were used to verify the quality and integrity of the imported data.

---

## Row Count

```sql
SELECT COUNT(*) AS total_employees
FROM employee_attrition;
```

Expected result:

```text
1,470 employees
```

---

## Duplicate Employee Numbers

```sql
SELECT
    "EmployeeNumber",
    COUNT(*) AS duplicate_count
FROM employee_attrition
GROUP BY "EmployeeNumber"
HAVING COUNT(*) > 1;
```

### Purpose

Ensures that each employee appears only once.

---

## Check Attrition Values

```sql
SELECT
    "Attrition",
    COUNT(*) AS employee_count
FROM employee_attrition
GROUP BY "Attrition"
ORDER BY employee_count DESC;
```

### Purpose

Validates the categorical values used to calculate attrition.

---

## Check Departments

```sql
SELECT
    "Department",
    COUNT(*) AS employee_count
FROM employee_attrition
GROUP BY "Department"
ORDER BY employee_count DESC;
```

### Purpose

Confirms department categories and employee distribution.

---

## Check Job Roles

```sql
SELECT
    "JobRole",
    COUNT(*) AS employee_count
FROM employee_attrition
GROUP BY "JobRole"
ORDER BY employee_count DESC;
```

---

## Check Missing Values

Missing-value checks were performed across relevant analytical fields to ensure that calculations were not unintentionally affected by NULL values.

---

# 03 — HR Business Questions

### File

`03_hr_business_questions.sql`

This script contains the main SQL analysis performed to answer the stakeholder questions.

---

# Question 1 — What Is the Overall Attrition Rate?

```sql
SELECT
    COUNT(*) AS total_employees,
    COUNT(CASE WHEN "Attrition" = 'Yes' THEN 1 END) AS attrition_count,
    ROUND(
        COUNT(CASE WHEN "Attrition" = 'Yes' THEN 1 END) * 100.0
        / COUNT(*),
        2
    ) AS attrition_rate
FROM employee_attrition;
```

### Business Purpose

Establishes the overall scale of employee turnover.

### Portfolio Finding

The dataset contains **1,470 employees**, with **237 employees classified as having left**, producing an overall attrition rate of approximately **16.12%**.

---

# Question 2 — Which Departments Lose the Most Employees?

```sql
SELECT
    "Department",
    COUNT(*) AS total_employees,
    COUNT(CASE WHEN "Attrition" = 'Yes' THEN 1 END) AS attrition_count,
    ROUND(
        COUNT(CASE WHEN "Attrition" = 'Yes' THEN 1 END) * 100.0
        / COUNT(*),
        2
    ) AS attrition_rate
FROM employee_attrition
GROUP BY "Department"
ORDER BY attrition_rate DESC;
```

### Business Purpose

Identifies departments with elevated employee turnover.

### Portfolio Finding

**Sales** recorded the highest departmental attrition rate at approximately **20.63%**.

---

# Question 3 — Which Job Roles Have the Highest Attrition?

```sql
SELECT
    "JobRole",
    COUNT(*) AS total_employees,
    COUNT(CASE WHEN "Attrition" = 'Yes' THEN 1 END) AS attrition_count,
    ROUND(
        COUNT(CASE WHEN "Attrition" = 'Yes' THEN 1 END) * 100.0
        / COUNT(*),
        2
    ) AS attrition_rate
FROM employee_attrition
GROUP BY "JobRole"
ORDER BY attrition_rate DESC;
```

### Business Purpose

Identifies roles that may require targeted retention investigation.

### Portfolio Finding

**Sales Representatives** recorded the highest attrition rate at approximately **39.76%**.

---

# Question 4 — Does Overtime Increase Attrition?

```sql
SELECT
    "OverTime",
    COUNT(*) AS total_employees,
    COUNT(CASE WHEN "Attrition" = 'Yes' THEN 1 END) AS attrition_count,
    ROUND(
        COUNT(CASE WHEN "Attrition" = 'Yes' THEN 1 END) * 100.0
        / COUNT(*),
        2
    ) AS attrition_rate
FROM employee_attrition
GROUP BY "OverTime"
ORDER BY attrition_rate DESC;
```

### Business Purpose

Measures the relationship between overtime and employee turnover.

### Portfolio Finding

Employees working overtime recorded an attrition rate of approximately **30.53%**, compared with **10.44%** among employees who did not work overtime.

This represents a difference of approximately **20 percentage points**.

---

# Question 5 — Does Salary Affect Employee Retention?

Salary bands were created to make compensation comparisons easier.

Example classification:

```sql
CASE
    WHEN "MonthlyIncome" < 5000 THEN 'Low'
    WHEN "MonthlyIncome" < 10000 THEN 'Medium'
    ELSE 'High'
END AS salary_band
```

The resulting salary bands were then analyzed against attrition.

```sql
SELECT
    CASE
        WHEN "MonthlyIncome" < 5000 THEN 'Low'
        WHEN "MonthlyIncome" < 10000 THEN 'Medium'
        ELSE 'High'
    END AS salary_band,
    COUNT(*) AS total_employees,
    COUNT(CASE WHEN "Attrition" = 'Yes' THEN 1 END) AS attrition_count,
    ROUND(
        COUNT(CASE WHEN "Attrition" = 'Yes' THEN 1 END) * 100.0
        / COUNT(*),
        2
    ) AS attrition_rate
FROM employee_attrition
GROUP BY salary_band
ORDER BY attrition_rate DESC;
```

### Portfolio Finding

The **Low Salary Band** recorded the highest attrition rate at approximately **21.76%**.

---

# Question 6 — Which Age Group Resigns the Most?

Age groups were created using a `CASE` expression.

```sql
CASE
    WHEN "Age" BETWEEN 18 AND 25 THEN '18–25'
    WHEN "Age" BETWEEN 26 AND 35 THEN '26–35'
    WHEN "Age" BETWEEN 36 AND 45 THEN '36–45'
    WHEN "Age" BETWEEN 46 AND 55 THEN '46–55'
    ELSE '56+'
END AS age_group
```

The groups were then compared using attrition rate.

### Portfolio Finding

The **18–25 age group** recorded the highest attrition rate at approximately **35.77%**.

---

# Question 7 — Does Work-Life Balance Influence Attrition?

```sql
SELECT
    "WorkLifeBalance",
    COUNT(*) AS total_employees,
    COUNT(CASE WHEN "Attrition" = 'Yes' THEN 1 END) AS attrition_count,
    ROUND(
        COUNT(CASE WHEN "Attrition" = 'Yes' THEN 1 END) * 100.0
        / COUNT(*),
        2
    ) AS attrition_rate
FROM employee_attrition
GROUP BY "WorkLifeBalance"
ORDER BY "WorkLifeBalance";
```

### Business Purpose

Investigates whether employees reporting lower work-life balance scores experience higher turnover.

### Portfolio Finding

Employees with the lowest Work-Life Balance score recorded an attrition rate of approximately **31.25%**.

---

# Question 8 — Does Job Satisfaction Influence Attrition?

```sql
SELECT
    "JobSatisfaction",
    COUNT(*) AS total_employees,
    COUNT(CASE WHEN "Attrition" = 'Yes' THEN 1 END) AS attrition_count,
    ROUND(
        COUNT(CASE WHEN "Attrition" = 'Yes' THEN 1 END) * 100.0
        / COUNT(*),
        2
    ) AS attrition_rate
FROM employee_attrition
GROUP BY "JobSatisfaction"
ORDER BY "JobSatisfaction";
```

### Portfolio Finding

Employees with the lowest Job Satisfaction score recorded an attrition rate of approximately **22.84%**, compared with approximately **11.33%** among employees with the highest satisfaction score.

---

# Question 9 — Which Employees Are at the Highest Risk?

A rule-based risk framework was used to segment employees based on selected attrition-related factors.

The factors included:

* Overtime
* Job Satisfaction
* Work-Life Balance
* Monthly Income
* Years Since Last Promotion

The resulting risk classifications were:

* Low Risk
* Medium Risk
* High Risk

The observed attrition rate was then calculated for each risk group.

### Portfolio Finding

The High Risk group recorded an observed attrition rate of approximately **36.96%**.

This compares with:

| Risk Level  | Observed Attrition Rate |
| ----------- | ----------------------: |
| Low Risk    |                   7.14% |
| Medium Risk |                  22.87% |
| High Risk   |                  36.96% |

The increasing attrition rate across the risk categories provides evidence that the rule-based framework is useful for **historical workforce segmentation**.

---
### Analytical Calculations

* Attrition rate
* Employee counts
* Average compensation
* Workforce segmentation
* Risk classification

---

# 📊 SQL-to-Power BI Validation

The SQL results were used as a validation layer for the Power BI dashboard.

The following metrics were cross-checked between SQL and Power BI:

* Total Employees
* Attrition Count
* Attrition Rate
* Department Attrition
* Job Role Attrition
* Overtime Attrition
* Salary Band Attrition
* Age Group Attrition
* Work-Life Balance Attrition
* Risk-Level Attrition

This helped ensure that the dashboard calculations were consistent with the underlying dataset.

---

# 💼 Business Value

The SQL analysis converts HR questions into measurable business metrics.

The analysis provides evidence for investigating:

* High-turnover departments
* High-turnover job roles
* Overtime-related attrition
* Compensation-related turnover
* Early-career employee attrition
* Employee satisfaction
* Work-life balance
* Career progression
* High-risk workforce segments

These findings were subsequently incorporated into the Power BI dashboard and HR recommendations.

---

# ⚠️ Analytical Limitations

The SQL analysis identifies **associations and patterns**, not causal relationships.

For example:

> A higher attrition rate among overtime employees does not prove that overtime alone caused employees to leave.

Other factors may interact with overtime, including:

* Job role
* Salary
* Age
* Job satisfaction
* Career progression
* Work-life balance

The risk analysis is also a **rule-based segmentation framework**, not a predictive machine-learning model.
---

## 👤 Skills Demonstrated

`PostgreSQL` · `SQL` · `Data Cleaning` · `Data Validation` · `Data Analysis` · `Conditional Aggregation` · `CASE Statements` · `Business Intelligence` · `HR Analytics` · `Business Problem Solving`
