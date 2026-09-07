# CAR-SALES-ANALYSIS

## Table of Content
- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Tools & Technologies](#tools-&-technologies)

### Project Overview
This project focuses on analyzing car sales data to uncover meaningful insights into sales performance, revenue generation, customer purchasing patterns, vehicle characteristics, and dealer performance.

### Objectives

The main objectives of this project are to:

- Analyze overall car sales performance.
- Calculate total revenue generated from vehicle sales.
- Identify the best-performing car manufacturers and models.
- Evaluate dealer and regional sales performance.
- Understand customer purchasing patterns and demographics.
- Compare average and highest vehicle prices.
- Examine sales trends over time.
- Identify high-value sales and potential business opportunities.

### Tools & Technologies
- **MySQL** — Data cleaning, transformation, querying, and analysis
- **SQL** — Data manipulation and business analysis
- **Excel** — Source dataset and initial data inspection

## Analysis
Before performing the analysis, the dataset was inspected for potential data quality issues.

#### Date Conversion

The Date column was initially stored as text rather than a proper SQL DATE data type.

The dates were converted using:
``` SQL
SET SQL_SAFE_UPDATES = 0;

UPDATE my_carss.`car sales.xlsx - car_data`
SET `Date` = Case 
when `Date` like '%/%' then date_format(STR_TO_DATE(`Date`,'%m/%d/%Y'), '%Y-%m-%d')
when `Date` like '%-%' then date_format(STR_TO_DATE(`Date`,'%Y-%m-%d'), '%Y-%m-%d')
ELSE null
END;

ALTER TABLE my_carss.`car sales.xlsx - car_data`
MODIFY COLUMN `Date`DATE;

set sql_safe_updates = 1;


