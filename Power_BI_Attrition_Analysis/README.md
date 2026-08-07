# 📊 Power BI Analysis — IBM HR Employee Attrition Analytics

## Overview

Microsoft Power BI was used to transform the cleaned IBM HR Employee Attrition dataset and SQL analysis into an interactive business intelligence dashboard for HR leadership.

The dashboard is designed around the HR Director's key business questions and provides an analytical journey from:

**Executive Overview → Department Analysis → Employee Experience → Compensation → Risk Analysis**

The objective is to move beyond simply reporting employee turnover and provide HR stakeholders with an interactive way to identify **where attrition is concentrated, which workforce characteristics are associated with higher attrition, and which employee segments may require further retention investigation.**

---

# 🎯 Business Problem

Employee turnover is increasing, and management wants to understand:

> **Why are employees leaving, which workforce groups experience the highest attrition, and where should HR focus retention efforts?**

The Power BI dashboard answers these questions by analyzing:

* Attrition
* Department
* Job role
* Demographics
* Compensation
* Overtime
* Employee satisfaction
* Work-life balance
* Career progression
* Tenure
* Risk segmentation

---

# 🛠️ Tools & Technologies

| Tool            | Purpose                                   |
| --------------- | ----------------------------------------- |
| **Power BI**    | Dashboard development and visualization   |
| **Power Query** | Data transformation and preparation       |
| **DAX**         | Measures and calculated columns           |
| **PostgreSQL**  | SQL analysis and data validation          |
| **Excel**       | Initial cleaning and exploratory analysis |

---

# 📂 Dataset

### Dataset

**IBM HR Analytics Employee Attrition & Performance**

### Records

**1,470 employees**

### Key analytical fields

* EmployeeNumber
* Age
* Attrition
* Department
* JobRole
* Gender
* MaritalStatus
* Education
* EducationField
* MonthlyIncome
* PercentSalaryHike
* StockOptionLevel
* OverTime
* JobSatisfaction
* EnvironmentSatisfaction
* WorkLifeBalance
* PerformanceRating
* YearsAtCompany
* YearsInCurrentRole
* YearsSinceLastPromotion
* YearsWithCurrManager

---

# 🔄 Power BI Development Workflow

```text
Excel / PostgreSQL
       ↓
Power Query
       ↓
Data Cleaning & Transformation
       ↓
Data Model
       ↓
Calculated Columns
       ↓
DAX Measures
       ↓
Dashboard Visuals
       ↓
Interactive Slicers
       ↓
HR Insights
       ↓
Recommendations
```

---

# 🧱 Data Model

The Power BI model was designed around the employee-level fact table.

### Fact Table

```text
FactEmployee
```

This table contains the employee-level records and measures used throughout the report.

### Analytical Dimensions

Where appropriate, analytical dimensions were created for:

* Department
* Job Role
* Education
* Age Group
* Tenure Band

The model supports filtering and analysis across different workforce segments.

---

# 📐 Core DAX Measures

## Total Employees

```DAX
Total Employees =
COUNT(FactEmployee[EmployeeNumber])
```

---

## Attrition Count

```DAX
Attrition Count =
CALCULATE(
    COUNT(FactEmployee[EmployeeNumber]),
    FactEmployee[Attrition] = "Yes"
)
```

---

## Active Employees

```DAX
Active Employees =
[Total Employees] - [Attrition Count]
```

---

## Attrition Rate

```DAX
Attrition Rate =
DIVIDE(
    [Attrition Count],
    [Total Employees],
    0
)
```

---

## Average Salary

```DAX
Average Salary =
AVERAGE(FactEmployee[MonthlyIncome])
```

---

## Average Age

```DAX
Average Age =
AVERAGE(FactEmployee[Age])
```

---

## Average Tenure

```DAX
Average Tenure =
AVERAGE(FactEmployee[YearsAtCompany])
```

---

## Average Job Satisfaction

```DAX
Average Job Satisfaction =
AVERAGE(FactEmployee[JobSatisfaction])
```

---

# 🔧 Calculated Columns

Several calculated columns were created to support segmentation and visualization.

---

## Age Group

Employees were grouped into meaningful age categories.

```DAX
Age Group =
SWITCH(
    TRUE(),
    FactEmployee[Age] <= 25, "18–25",
    FactEmployee[Age] <= 35, "26–35",
    FactEmployee[Age] <= 45, "36–45",
    FactEmployee[Age] <= 55, "46–55",
    "56+"
)
```

---

# 💰 Salary Band

Monthly income was segmented into three analytical categories.

```DAX
Salary Band =
IF(
    FactEmployee[MonthlyIncome] < 5000,
    "Low",
    IF(
        FactEmployee[MonthlyIncome] < 10000,
        "Medium",
        "High"
    )
)
```

---

# ⏳ Tenure Band

Employees were categorized according to their years at the company.

| YearsAtCompany | Tenure      |
| -------------- | ----------- |
| 0–2            | New         |
| 3–5            | Growing     |
| 6–10           | Experienced |
| 10+            | Veteran     |

Example DAX:

```DAX
Tenure Band =
SWITCH(
    TRUE(),
    FactEmployee[YearsAtCompany] <= 2, "New",
    FactEmployee[YearsAtCompany] <= 5, "Growing",
    FactEmployee[YearsAtCompany] <= 10, "Experienced",
    "Veteran"
)
```

---

# 📊 Dashboard Pages

The report contains five pages.

---

# 01 — Executive Overview

### Business Question

> **What is happening with employee attrition?**

The Executive Overview provides HR leadership with a high-level snapshot of workforce health.

### KPI Cards

* Total Employees
* Attrition Count
* Attrition Rate
* Active Employees
* Average Salary
* Average Age
* Average Tenure
* Average Job Satisfaction

### Core Visuals

* Attrition by Department
* Attrition by Gender
* Attrition by Age Group
* Attrition by Marital Status

### Slicers

* Department
* Job Role
* Gender
* Age Group
* Salary Band
* Overtime
* Marital Status

### Purpose

Provide a fast executive-level understanding of the organization's attrition position.

---

# 02 — Department Analysis

### Business Question

> **Which departments and job roles experience the greatest employee turnover?**

### Core Visuals

* Attrition by Department
* Attrition by Job Role
* Attrition by Education Field
* Attrition by Business Travel

### Analysis

This page allows HR to identify workforce segments where attrition is disproportionately high.

### Key Finding

The **Sales department** recorded the highest departmental attrition rate at approximately **20.63%**.

The **Sales Representative** role was particularly notable, with an attrition rate of approximately **39.76%**.

### Business Implication

HR should investigate whether workload, overtime, compensation, career progression, or employee experience contribute to the elevated turnover within these groups.

---

# 03 — Employee Experience

### Business Question

> **Does employee experience influence attrition?**

This page examines employee satisfaction and workplace experience.

### Core Visuals

* Job Satisfaction vs Attrition
* Environment Satisfaction vs Attrition
* Work-Life Balance vs Attrition
* Relationship Satisfaction vs Attrition
* Performance Rating vs Attrition

### Key Finding

Employees with the lowest Job Satisfaction score recorded an attrition rate of approximately **22.84%**, compared with approximately **11.33%** among employees with the highest satisfaction score.

Employees with the lowest Work-Life Balance score recorded an attrition rate of approximately **31.25%**.

### Business Implication

Employee experience should be considered alongside compensation and workload when developing retention initiatives.

---

# 04 — Compensation Analysis

### Business Question

> **Does compensation influence employee retention?**

The Compensation page investigates the relationship between employee income, salary progression, stock options, and attrition.

### Core Visuals

* Attrition by Salary Band
* Monthly Income Distribution
* Average Salary by Department
* Stock Option Level vs Attrition
* Percent Salary Hike vs Attrition

### Salary Segmentation

Employees were divided into:

* Low
* Medium
* High

### Key Finding

The **Low Salary Band** recorded the highest attrition rate at approximately **21.76%**.

### Business Implication

HR may need to investigate compensation competitiveness and internal pay equity for affected employee groups.

However, compensation should not be viewed in isolation because salary interacts with other factors such as overtime, job role, tenure, and employee satisfaction.

---

# 05 — Risk Analysis

### Business Question

> **Which employee segments meet multiple attrition-related risk criteria?**

The Risk Analysis page combines selected employee characteristics into a rule-based risk segmentation framework.

### Core Visuals

* Overtime vs Attrition
* Attrition by Tenure Band
* Years Since Last Promotion
* Risk Factor Analysis
* Risk Level Distribution
* High-Risk Employee Table

---

# ⚠️ Risk Scoring Framework

The project uses a rule-based framework based on selected attrition-related characteristics.

Risk factors include:

* Overtime
* Job Satisfaction
* Work-Life Balance
* Monthly Income
* Years Since Last Promotion

The framework assigns points according to predefined conditions and classifies employees into:

* Low Risk
* Medium Risk
* High Risk

### Important Note

The risk score is an **analytical segmentation framework** developed for this portfolio project.

It is **not a machine-learning prediction model** and should not be interpreted as an individual employee's probability of leaving.

---

# 📈 Risk Analysis Results

The observed attrition rates by risk group were:

| Risk Level  | Observed Attrition |
| ----------- | -----------------: |
| Low Risk    |          **7.14%** |
| Medium Risk |         **22.87%** |
| High Risk   |         **36.96%** |

The High Risk segment therefore recorded a substantially higher historical attrition rate than the Low Risk segment.

### Business Implication

The risk framework can help HR prioritize workforce segments for further investigation rather than applying broad retention interventions across the entire organization.

---

# 🎛️ Dashboard Slicers

The report uses slicers to allow HR stakeholders to investigate different workforce segments.

### Primary Slicers

* Department
* Job Role
* Gender
* Age Group
* Salary Band
* Tenure Band
* Overtime
* Marital Status

### Example Analysis

An HR Director can select:

```text
Department → Sales
        ↓
Job Role → Sales Representative
        ↓
Overtime → Yes
        ↓
Salary Band → Low
```

This allows the dashboard to dynamically reveal how attrition changes within a specific workforce segment.

---

# 📊 Key Dashboard Findings

The Power BI analysis identified several important patterns.

### Overall Attrition

**16.12%**

237 of the 1,470 employees were classified as having left.

---

### Highest Department Attrition

**Sales — 20.63%**

---

### Highest Job Role Attrition

**Sales Representative — 39.76%**

---

### Overtime

**30.53% attrition among overtime employees**

versus

**10.44% among non-overtime employees**

---

### Highest Salary Band Attrition

**Low Salary Band — 21.76%**

---

### Highest Age Group Attrition

**18–25 — 35.77%**

---

### Employee Experience

Lowest Job Satisfaction:

**22.84% attrition**

Highest Job Satisfaction:

**11.33% attrition**

---

### Risk Segmentation

High Risk:

**36.96% attrition**

Medium Risk:

**22.87% attrition**

Low Risk:

**7.14% attrition**

---

# 💡 HR Recommendations

Based on the analysis, the following areas should be considered for further HR investigation.

## 1. Review Overtime

Employees working overtime demonstrate substantially higher observed attrition.

HR should investigate:

* Workload distribution
* Staffing levels
* Burnout
* Scheduling
* Compensation for overtime

---

## 2. Investigate Sales Representative Attrition

The Sales Representative role has one of the most pronounced attrition patterns.

HR should investigate:

* Workload
* Compensation
* Career progression
* Management support
* Job satisfaction

---

## 3. Strengthen Early-Career Retention

The 18–25 age group demonstrates substantially higher attrition.

Potential interventions include:

* Mentorship
* Career development
* Training
* Clear promotion pathways
* Early-career engagement programs

---

## 4. Review Compensation

The Low Salary Band has a higher observed attrition rate.

HR could investigate:

* Internal pay equity
* Market competitiveness
* Salary progression
* Performance-based compensation

---

## 5. Improve Employee Experience

Lower satisfaction and work-life balance scores are associated with higher observed attrition.

HR should consider:

* Employee engagement initiatives
* Manager training
* Workload reviews
* Flexible work arrangements where appropriate
* Regular employee feedback

---

## 6. Prioritize High-Risk Segments

Rather than treating every employee group equally, HR can use the risk framework to identify segments that warrant deeper investigation.

---

# 📌 Business Story

The dashboard follows a structured HR decision-making journey:

```text
16.12% Overall Attrition
          ↓
Sales: 20.63%
          ↓
Sales Representatives: 39.76%
          ↓
Overtime Employees: 30.53%
          ↓
Low Salary Band: 21.76%
          ↓
18–25 Age Group: 35.77%
          ↓
High Risk Segment: 36.96%
```

This progression allows HR leadership to move from:

**"How large is the problem?"**

to:

**"Where is it concentrated?"**

to:

**"Which factors are associated with higher attrition?"**

and finally:

**"Which workforce segments should we investigate first?"**

---

# ⚠️ Analytical Limitations

The Power BI dashboard identifies patterns and associations within the dataset.

It does not establish causation.

For example:

> Higher attrition among overtime employees does not prove that overtime alone caused employees to leave.

Employee turnover may be influenced by multiple interacting factors.

The dataset is also historical and static, meaning the dashboard does not represent a live HR information system.

The risk framework is rule-based and should not be interpreted as a predictive attrition model.

---

# 📁 Power BI Repository Structure

```text
04_PowerBI/
│
├── IBM_HR_Attrition_Dashboard.pbix
│
├── Dashboard_Screenshots/
│   ├── 01_Executive_Overview.png
│   ├── 02_Department_Analysis.png
│   ├── 03_Employee_Experience.png
│   ├── 04_Compensation_Analysis.png
│   └── 05_Risk_Analysis.png
│
└── README.md
```

---

# 🔗 Project Navigation

| Stage                      | Repository Folder   |
| -------------------------- | ------------------- |
| Data Preparation           | `01_Data`           |
| Excel Analysis             | `02_Excel_Analysis` |
| SQL Analysis               | `03_SQL_Analysis`   |
| **Power BI Analysis**      | `04_PowerBI`        |
| DAX                        | `05_DAX`            |
| Insights & Recommendations | `06_Insights`       |

---

# 🚀 Future Improvements

Future versions of this project could include:

* Predictive attrition modeling using Python
* Machine learning-based attrition probability
* Employee-level prediction scoring
* Model performance evaluation
* Automated Power BI refresh
* Live HRIS integration
* Workforce forecasting
* Retention ROI analysis

---
