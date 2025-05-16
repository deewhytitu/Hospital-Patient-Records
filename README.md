# Hospital Patient Readmission Analysis (SQL Project)

## Table of Contents

- [Project Overview](#project-overview)
- [Business Questions or Objectives](#business-questions-or-objectives)
- [Dataset](#dataset)
- [Tools](#tools)
- [SQL Queries](#sql-queries)
- [Key Findings](#key-findings)
- [Recommendations](#recommendations)

### Project Overview

This Project analyzes patient readmission trends from a hospital's historical records (2011-2022) using Microsoft SQL Server. The objective is to to identify key patterns in pateint admissions, readmissions, and associated healthcare costs to support strategic decision-making.

### Business Questions or Objectives

1. How many patients have been admitted or readmitted over time?
2. How long are patients staying in the hospital, on average?
3. How much is the average cost per visit?
4. How many procedures are covered by insurance?

### Dataset

Hospital Patient Records: The dataset was gotten from Maven Analytics Data Playground. Dataset contains key tables: Encounters, Organizations, Patients, Payers, & Procedures.

### Tools
- Excel - Data Cleaning
- SQL - Data Analysis
- Power BI - Creating visuals

### SQL Queries

- How many patients have been admitted by year?

Query: 
SELECT 
     YEAR(CAST(Start AS DATETIME)) AS Admission_year, 
    COUNT(DISTINCT Patient) AS TotalPatientsAdmitted
FROM encounters
WHERE EncounterClass = 'inpatient' 
GROUP BY YEAR(CAST(Start AS DATETIME))
ORDER BY Admission_year;

Insight: The hospital saw a steady increase in admissions from 2011 to 2020 with a peak in 2019 and notable dip in some mid-years.

- How many patients have been readmitted by year?

Query:
WITH patient_admissions AS (
    SELECT 
        patient,
		YEAR(CAST(start AS DATE)) AS admission_year,
        COUNT(*) AS admissions_in_year
    FROM encounters
    GROUP BY patient, YEAR(CAST(start AS DATE))
)
SELECT 
    admission_year,
    COUNT(*) AS patients_readmitted
FROM patient_admissions
WHERE admissions_in_year > 1
GROUP BY admission_year
ORDER BY admission_year;

Insights: The hospital saw a sharp peak in number of readmission in 2020 (550 patients) 

- How long are patients staying in the hospital on average?

Query:
SELECT 
    AVG(DATEDIFF(DAY, CAST(Start AS DATETIME), CAST(Stop AS DATETIME))) AS AvgLengthOfStay
FROM encounters
WHERE EncounterClass = 'inpatient';

Insight: The average length of stay of patients in the hospital is one day.

- How much is the average cost per visit?

Query:
SELECT AVG(Total_claim_cost) 
FROM encounters;

Insight: The average cost of each hospital visit is $3,640

- How many procedures are covered by insurance?

Query:
SELECT COUNT(*) coveredprocedures
FROM encounters
WHERE PAYER_COVERAGE > 0;

Insight: 14,305 procedures are covered by insurance

 
### Key Findings
- Readmissions peaked in 2020, possibly due to complications related to chronic cases.
- Average length of stay remained consistent at 1 day.
- High readmission rate (75%) suggests the need for better post-discharge care.

### Recommendations
- The hospital should consider exploring causes of readmission (e.g discharge protocols, follow-up care, comorbidities).
- Reducing readmissions could result in major savings.
- Need to evaluate if length of stay is optimal or if patients are being discharged prematurely.    
