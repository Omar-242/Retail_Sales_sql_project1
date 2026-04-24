# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis

**Database**: sql_project_p1

This project demonstrates basic SQL skills used in data analysis, such as exploring data, cleaning it, and answering business questions. The dataset is based on retail sales transactions and is analyzed using PostgreSQL.

The goal is to practice real-world SQL techniques and understand how data can be used to generate meaningful business insights.

---

## Objectives

1. Create and set up a retail sales database
2. Clean the dataset by handling missing values
3. Perform exploratory data analysis (EDA)
4. Answer business questions using SQL queries

---

## Project Structure

### 1. Database Setup

The first step is creating a database and defining the structure of the retail sales table.

```sql
CREATE DATABASE sql_project_p1;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);

```

---

### 2. Data Exploration & Cleaning

Before analysis, the dataset is explored and cleaned to ensure accuracy.

* Total number of records
* Unique customers
* Available product categories
* Checking and removing null values

```sql
SELECT COUNT(*) FROM retail_sales;

SELECT 
COUNT(DISTINCT customer_id) 
FROM retail_sales;

-- Distinct coustomer 155

SELECT 
DISTINCT category 
FROM retail_sales;

-- Distinct product category 3

SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

-- Deleted records containing null values
```

---

### 3. Data Analysis & Business Questions

Below are the SQL queries used to extract insights from the dataset.

---

**1. Retrieve all sales made on '2022-11-05'**

```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

---

**2. Transactions in Clothing category with quantity > 4 in Nov-2022**

```sql
SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4
```

---

**3. Total sales per category**

```sql
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1
```

---

**4. Average age of Beauty category customers**

```sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'
```

---

**5. Transactions with total sale greater than 1000**

```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000
```

---

**6. Total transactions by gender and category**

```sql
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP 
    BY 
    category,
    gender
ORDER BY 1
```

---

**7. Best selling month in each year (based on average sales)**

```sql
SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1
```

---

**8. Top 5 customers by total sales**

```sql
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```

---

**9. Unique customers per category**

```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category
```

---

**10. Orders by shift (Morning, Afternoon, Evening)**

```sql
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

---

## Key Findings

* Customers belong to different age groups and shop across multiple categories like Clothing and Beauty
* Some transactions are high-value (above 1000), showing premium purchases
* Sales vary across months, indicating seasonal trends
* Top customers and popular categories were identified

---

## Reports Generated

* Sales summary across categories and customers
* Monthly trend analysis of sales performance
* Customer insights based on spending behavior

---

## Conclusion

This project is a beginner-level SQL analysis exercise that demonstrates how raw data can be transformed into meaningful business insights. It covers database setup, data cleaning, and analytical queries that help understand customer behavior and sales trends.

---

