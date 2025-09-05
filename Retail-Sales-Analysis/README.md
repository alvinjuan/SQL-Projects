# Retail Sales Analysis SQL Project

## Project Overview

**Database**: `p1_retail_db`  
**Tech Stack**: SQL (PostgreSQL/MySQL compatible)

---

## Objectives

1. **Database Setup**: Create and populate a retail sales database.  
2. **Data Cleaning**: Identify and remove invalid or null records.  
3. **Exploratory Data Analysis (EDA)**: Perform initial analysis to understand the dataset.  
4. **Business Insights**: Use SQL to answer key business questions.  
5. **Portfolio-Ready Project**: Build a case study suitable for **data analytics interviews**.

---

## Project Structure

### 1. Database Setup
Create the database and the main table `retail_sales` with columns for transaction ID, sale date/time, customer demographics, product category, and sales metrics.

```sql
-- SQL Retail Sales Analysis
CREATE DATABASE sql_project_p2;

-- Create TABLE
DROP TABLE IF EXISTS retail_sales;
CREATE TABLE retail_sales 
(
	transactions_id	INT PRIMARY KEY,
	sale_date DATE,
	sale_time TIME,
	customer_id INT,
	gender VARCHAR(7),
	age INT,
	category VARCHAR(12),
	quantity INT,
	price_per_unit FLOAT,
	cogs FLOAT,
	total_sale FLOAT
);
```

---

### 2. SQL Data Cleaning & Exploration
- **Record Count**: Total rows in the dataset  
- **Unique Customers**: Count of distinct customers  
- **Unique Categories**: Identify distinct product categories  
- **Null Check & Removal**: Remove rows with missing values  

```sql
-- Data Exploration
SELECT * FROM retail_sales;

-- How many sales we have?
SELECT COUNT(*) AS total_sales
FROM retail_sales;

-- How many unique customers we have?
SELECT COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales;

-- How many categories we have?
SELECT COUNT(DISTINCT category) AS count_of_categories
FROM retail_sales;

-- What are the categories?
SELECT DISTINCT category AS count_of_categories
FROM retail_sales;

-- Delete Null Values
DELETE FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
      gender IS NULL OR age IS NULL OR category IS NULL OR 
      quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

---

### 3. SQL Queries for Data Analytics & Business Insights

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05.**  
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022.**  
```sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**  
```sql
SELECT
	category,
	SUM(total_sale) AS total_sale,
	COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**  
```sql
SELECT 
	ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**  
```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**  
```sql
SELECT 
	category,
	gender,
	COUNT(*) AS total_transactions
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year.**  
```sql
WITH month_ranking_per_year AS 
(
	SELECT
		EXTRACT(YEAR FROM sale_date) as year,
		EXTRACT(MONTH FROM sale_date) as month,
		AVG(total_sale) as avg_sale,
		RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
	FROM retail_sales
	GROUP BY 1, 2
) 
SELECT
	year,
	month,
	avg_sale
FROM month_ranking_per_year
WHERE rank = 1;
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales.**  
```sql
SELECT
  customer_id,
  SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**  
```sql
SELECT
  category,
  COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category;
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17).**  
```sql
WITH hourly_sale AS 
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
	COUNT(*) AS total_amount_orders
FROM hourly_sale
GROUP BY shift;
```

---

## Findings

- **Customer Demographics**: Wide age distribution across genders and categories.  
- **High-Value Transactions**: Several sales > 1000, showing premium purchases.  
- **Seasonality**: Certain months outperform others, useful for forecasting.  
- **Top Customers**: Identified highest-spending customers and loyal segments.  
- **Category Trends**: Clothing and Beauty were major contributors to sales.  

---

## Skills Learned

- SQL **data cleaning & preprocessing**  
- Writing **aggregate queries** (SUM, AVG, COUNT)  
- Using **JOINS, GROUP BY, and window functions**  
- Performing **business-driven analysis with SQL**  
- Building a **portfolio-ready data analytics case study**  

---

## Reports

- **Sales Summary**: Total sales and order counts across categories.  
- **Customer Insights**: Top customers and demographic analysis.  
- **Trend Analysis**: Peak sales months and shifts.  

---

## How to Use

1. **Clone the Repository**  
   ```bash
   git clone <your-repo-link>
   ```
2. **Set Up the Database**  
   Run the `database_setup.sql` script to create and populate the database.  
3. **Run the Queries**  
   Use the provided SQL scripts to perform analysis.  
4. **Modify & Explore**  
   Extend queries or adapt them for interview prep and analytics practice.  
