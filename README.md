# Retail Sales Analysis SQL Project
## Project Overview

**Project Title**: Retail Sales Analysis   
**Database**: `sql_project_p1_db`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

5. ## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `sql_project_p1_db`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.


--SQL Retail Sales Analysis
CREATE DATABASE sql_project_p1;


--CREATE TABLE
DROP TABLE IF EXISTS retail_sales;
 
CREATE TABLE retail_sales(

     transactions_id INT PRIMARY KEY,
	 sale_date	DATE,
	 sale_time  TIME,
	 customer_id INT,	
	 gender	VARCHAR(10),
	 age INT,
	 category	VARCHAR(20),
	 quantity	INT,
	 price_per_unit	 FLOAT,
	 cogs	FLOAT,
	 total_sale FLOAT
);

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```
select * from retail_sales;

select * from retail_sales
LIMIT 10; 

--TOTAL COUNT
select 
  count(*)
from retail_sales;
```
```
--DATA CLEANING

--FINDINGS NULL 
select * from retail_sales
WHERE 
    transactions_id IS NULL
	OR 
	sale_date IS NULL
	OR
	sale_time IS NULL
	OR 
	customer_id IS NULL
	OR 
    gender IS NULL
	OR 
	category IS NULL
	OR 
	cogs IS NULL
	OR
	total_sale IS NULL

--DELETE NULL
DELETE FROM retail_sales
WHERE 
    transactions_id IS NULL
	OR 
	sale_date IS NULL
	OR
	sale_time IS NULL
	OR 
	customer_id IS NULL
	OR 
    gender IS NULL
	OR 
	category IS NULL
	OR 
	cogs IS NULL
	OR
	total_sale IS NULL;
```


--DATA EXPLORATION

The following SQL queries were developed to answer specific business questions:
```
--Q. How many sales we have ?

SELECT COUNT(*) as total_sale FROM retail_sales;

SELECT * FROM retail_sales
limit 10

--Q. How many unique customers we have ?

SELECT COUNT(DISTINCT customer_id) as total_sale FROM retail_sales;


select distinct customer_id FROM retail_sales;

select distinct category from retail_sales;


--<< Data Analysis & Key Problems >>

-- Q. Write a SQL query to retrieve all columns for sales made on '2022-11-05

SELECT * FROM retail_sales
WHERE sale_date='2022-11-05';


--Q. Write a SQL query to find all transactions where the total_sale is greater than 1000.

SELECT * FROM retail_sales
where total_sale>1000;


--Q. SUM of total sales of Clothing category

SELECT SUM(total_sale) as net_sale 
from retail_sales
WHERE category='Clothing'; --( = comparing only one value)....


SELECT * FROM retail_sales;

SELECT SUM(total_sale) from retail_sales
WHERE category IN ('Clothing','Beauty','Electronics'); 


SELECT MAX(total_sale) FROM retail_sales;
SELECT MIN(total_sale) FROM retail_sales;




--Q. Write a SQL query to retrieve all transactions where the category is 'Clothing' 
--and the quantity sold is more than 4 in the month of Nov-2022.

SELECT *
FROM retail_sales
where category='Clothing'
     AND 
     TO_CHAR (sale_date,'YYYY-MM') = '2022-11'  --(TO_CHAR = CONVERT NO, DATES INTO TEXT)
     AND 
	 quantity >=4
	 


--Q.  Write a SQL query to calculate the total sales (total_sale) for each category.

SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1


--Q. Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category
--SOLUTION 1
SELECT AVG(age) as avg_age 
from retail_sales
WHERE category='Beauty'

--SOLUTION 2
SELECT 
ROUND(AVG(age),2) as avg_age 
from retail_sales
WHERE category='Beauty'


--Q. Write a SQL query to find the total number of transactions (transaction_id)
--made by each gender in each category.

SELECT 
    category,
    gender,
COUNT(*) AS total_transactions
FROM retail_sales
GROUP BY 
    category,
    gender
ORDER BY 1


--Q. Write a SQL query to find the top 5 customers based on the highest total sales.

SELECT * FROM retail_sales

SELECT 
     customer_id,
	 SUM(total_sale) as total_sales
	 from retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5


--Q. Write a SQL query to find the number of unique customers who purchased items from each category.

SELECT
category,
COUNT(DISTINCT(customer_id)) as unique_customers
from retail_sales
GROUP BY category;


--Q. Write a SQL query to calculate the average sale for each month. Find out best selling month in each year.
SELECT * FROM(
SELECT 
      EXTRACT(YEAR FROM sale_date) as year,
	  EXTRACT(MONTH FROM sale_date) as month,
	  AVG(total_sale) as avg_sale,
	  RANK()OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
From retail_sales
GROUP BY year,month
) as t1
WHERE RANK=1

--Q. Write a SQL query to create each shift and number of orders 
--(Example Morning <12, Afternoon Between 12 & 17, Evening >17)

WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift
```

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries.
The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

