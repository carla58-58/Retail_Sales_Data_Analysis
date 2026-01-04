# Retail_Sales_Data_Analysis

Complete PostgreSQL Data Analysis: Schema → Cleaning → 12 Advanced Business Queries

📊 Project Overview
Full SQL workflow: Database design → Data quality → Business intelligence queries using CTEs, window functions, and aggregations.

🏗️ Database Schema

CREATE TABLE retail_sales (
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

🧹 Data Cleaning Pipeline

-- Data Quality Check (9 columns)
SELECT * FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR ...;

-- Remove null records
DELETE FROM retail_sales WHERE sale_date IS NULL OR ...;

🔥 Key Business Queries Implemented
Basic Explorations
Total records, unique customers, categories

Sample data preview (LIMIT 10)

Date & Category Analysis
Sales on specific date: '2022-11-05'

Clothing sales >4 units (Nov 2022)

Total sales + orders per category

Customer Insights

- Average age: Beauty category customers
- Top 5 customers by total sales
- Unique customers per category

Advanced Analytics

-- Best selling month per year (Window Functions)
SELECT year, month, avg_sale
FROM (
    SELECT EXTRACT(YEAR FROM sale_date) as year,
           EXTRACT(MONTH FROM sale_date) as month,
           AVG(total_sale) as avg_sale,
           RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) 
                      ORDER BY AVG(total_sale) DESC) as rank
    FROM retail_sales GROUP BY 1, 2
) t1 WHERE rank = 1;


-- Sales by shift (CTE + CASE)
WITH hourly_sale AS (
    SELECT *,
        CASE WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
             WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
             ELSE 'Evening' END as shift
    FROM retail_sales
)
SELECT shift, COUNT(*) as total_orders FROM hourly_sale GROUP BY shift;

📈 Business Questions Answered
High-value sales: total_sale > 1000

Gender/Category breakdown: Transactions by demographics

Category performance: Revenue + unique customers

💻 Technologies

🗄️ PostgreSQL (pgAdmin)
🔍 SQL (CTEs, Window Functions, Aggregations)
📊 Data Cleaning & EDA

🚀 Full Workflow

1. CREATE TABLE → Load data
2. Data Quality → Remove nulls  
3. 12 Business Queries → Validate results
4. Document insights for stakeholders

Complete script: retail_sales.sql

Portfolio Skills Demonstrated:
✅ Schema design | ✅ Data cleaning | ✅ CTEs | ✅ Window functions | ✅ Business analytics

