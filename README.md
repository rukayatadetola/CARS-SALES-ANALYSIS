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

### Data Cleaning
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
```

### Explorary Analysis
1. **Total Cars Sold**
 ```SQL
   SELECT COUNT(*) AS Count_Cars_Sold
   FROM my_carss.`car sales.xlsx - car_data`;
```
2. **Total Revenue**
```SQL
SELECT sum(`Price ($)`) AS Total_Revenue
From my_carss.`car sales.xlsx - car_data`;
```
3. **Average Car Price**
``` SQL
SELECT 
    ROUND(AVG(`Price($)`), 2) AS Average_Car_Price
FROM my_carss.`car sales.xlsx - car_data`;
```

4. **Top Car Manufacturers by Revenue**
```SQL
SELECT 
    Company,
    COUNT(*) AS Cars_Sold,
    SUM(`Price($)`) AS Total_Revenue
FROM my_carss.`car sales.xlsx - car_data`
GROUP BY Company
ORDER BY Total_Revenue DESC;
```
5. **Top 10 Best-Selling Models**
```SQL
SELECT
 Model,
count(*) AS Unit_sold
from my_carss.`car sales.xlsx - car_data`
group by Model
order by Unit_sold desc
limit 10;
```
6.  **Dealer Performance Analysis**
``` SQL
SELECT
Dealer_Name,
count(*) AS Unit_Sold,
sum(`Price ($)`) AS Total_Revenue
from my_carss.`car sales.xlsx - car_data`
group by Dealer_Name
order by Total_Revenue desc;
```
7. **Regional Sales Analysis**
```SQL
SELECT 
    Dealer_Region,
    COUNT(*) AS Cars_Sold,
    SUM(`Price($)`) AS Total_Revenue,
    ROUND(AVG(`Price($)`), 2) AS Average_Car_Price
FROM my_carss.`car sales.xlsx - car_data`
GROUP BY Dealer_Region
ORDER BY Total_Revenue DESC;
```
8. **Top 10 most expensive car models**
```SQL
SELECT
Model, Company,
Max(`price ($)`) AS Highest_Price
from my_carss.`car sales.xlsx - car_data`
group by Model, Company
order by Highest_Price
limit 10;
```
9. **Sales by Gender**
```SQL
SELECT 
    Gender,
    COUNT(*) AS Cars_Purchased,
    SUM(`Price($)`) AS Total_Spending,
    ROUND(AVG(`Price($)`), 2) AS Average_Spending
FROM my_carss.`car sales.xlsx - car_data`
GROUP BY Gender
ORDER BY Total_Spending DESC;
```
10. **Monthly Sales Trend**
```SQL
SELECT 
    YEAR(`Date`) AS Sales_Year,
    MONTH(`Date`) AS Sales_Month,
    COUNT(*) AS Cars_Sold,
    SUM(`Price($)`) AS Total_Revenue
FROM my_carss.`car sales.xlsx - car_data`
GROUP BY 
    YEAR(`Date`),
    MONTH(`Date`)
ORDER BY 
    Sales_Year,
    Sales_Month;
```
