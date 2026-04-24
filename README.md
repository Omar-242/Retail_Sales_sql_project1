# Retail Sales SQL Analysis

## 📌 Project Overview

This project analyzes retail sales data using SQL to find useful insights about sales trends, customer behavior, and product performance.

---

## 🛠 Tools Used

* PostgreSQL (pgAdmin 4)
* SQL

---

## 📊 Dataset

* Retail sales dataset
* Includes sales amount, categories, customer age, and dates etc

---

## ❓ Key Questions

* What are the monthly sales trends?
* Which month has the highest average sales?
* Who are the top customers?
* Which product categories perform best?

---

## 🔍 Key Findings

* Sales come from different age groups and categories like Clothing and Beauty
* Some transactions are high-value (above 1000)
* Sales change across months, showing peak seasons
* Top customers and best categories were identified

---

## 📈 Sample Query

```sql
SELECT 
    EXTRACT(YEAR FROM sale_date) AS year,
    EXTRACT(MONTH FROM sale_date) AS month,
    AVG(total_sale) AS avg_sale
FROM retail_sales
GROUP BY 1, 2;
```

---

## 📸 Output Example


---

## ✅ Conclusion

This project shows basic SQL skills like data analysis, aggregation, and generating business insights from raw data.
