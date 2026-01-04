# Retail_Sales_Data_Analysis

**Complete PostgreSQL Analysis: Schema → Cleaning → 12 Advanced Business Queries**

## 📊 Project Overview
Full SQL workflow: Database design → Data quality → Business intelligence using CTEs, window functions, aggregations.

## 🏗️ Database Schema

```sql
CREATE TABLE retail_sales (
    transactions_id INT PRIMARY KEY,
    sale_date DATE, sale_time TIME,
    customer_id INT, gender VARCHAR(10),
    age INT, category VARCHAR(35),
    quantity INT, price_per_unit FLOAT,
    cogs FLOAT, total_sale FLOAT
);

🧹 Data Cleaning

-- Check nulls across 9 columns
SELECT * FROM retail_sales 
WHERE sale_date IS NULL OR sale_time IS NULL OR ...;

-- Remove null records
DELETE FROM retail_sales WHERE sale_date IS NULL OR ...;

🔥 Key Queries (12 Total)

1-4. Basic EDA
Total records, unique customers, categories

Sample data (LIMIT 10)

5-8. Business Analysis
Specific date sales (2022-11-05)

Clothing >4 units (Nov 2022)

Category totals (revenue + orders)

Beauty category avg age

9-12. Advanced SQL

-- Best month per year (Window Functions)
SELECT year, month, avg_sale FROM (
    SELECT EXTRACT(YEAR FROM sale_date) as year,
           AVG(total_sale) as avg_sale,
           RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) 
                      ORDER BY AVG(total_sale) DESC) as rank
    FROM retail_sales GROUP BY 1
) t1 WHERE rank = 1;


-- Sales by shift (CTE)
WITH hourly_sale AS (
    SELECT CASE 
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening' 
    END as shift FROM retail_sales
)
SELECT shift, COUNT(*) as total_orders FROM hourly_sale GROUP BY shift;

💻 Technologies

🗄️ PostgreSQL/pgAdmin
🔍 SQL (CTEs, Window Functions, Aggregations)
📊 Data Cleaning + EDA

🚀 Workflow
CREATE TABLE → Load retail data

Data Quality → Remove nulls

12 Queries → Business insights

Validate → Stakeholder-ready results

Full script: retail_sales.sql

Skills: Schema design | Data cleaning | CTEs | Window functions | Business SQL

