# Retail Sales Analysis SQL Project

## Project Overview

- **Project Title**: Retail Sales Analysis
- **Level**: Beginner
- **Database**: `p1_retail_db`

This project demonstrates essential SQL skills and techniques for data analysis, focusing on exploring, cleaning, and analyzing retail sales data. Ideal for those starting out in data analysis, this project provides a strong foundation in SQL.

## Objectives

1. **Set Up Database**: Create and populate the retail sales database with provided sales data.
2. **Data Cleaning**: Identify and remove records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Understand the dataset through basic exploratory analysis.
4. **Business Analysis**: Use SQL to answer key business questions and derive insights.

---

## Project Structure

### 1. Database Setup

1. **Database Creation**: The project begins by creating a database named `p1_retail_db`.
2. **Table Creation**: A table named `retail_sales` is created to store sales data with the following structure:

    ```sql
    CREATE DATABASE p1_retail_db;

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
    ```

### 2. Data Exploration & Cleaning

- **Record Count**: Find the total number of records in the dataset.
    ```sql
    SELECT COUNT(*) FROM retail_sales;
    ```
- **Customer Count**: Determine the unique customer count.
    ```sql
    SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
    ```
- **Category Count**: Identify unique product categories.
    ```sql
    SELECT DISTINCT category FROM retail_sales;
    ```
- **Null Value Check and Removal**: Check for any null values and remove records with missing data.
    ```sql
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
    ```

### 3. Data Analysis & Findings

The following SQL queries were used to answer specific business questions:

- **Sales on a Specific Date**: Retrieve all columns for sales made on '2022-11-05'.
    ```sql
    SELECT * FROM retail_sales WHERE sale_date = '2022-11-05';
    ```

- **High-Quantity Sales in a Category**: Transactions in the 'Clothing' category with quantities sold greater than 4 in Nov-2022.
    ```sql
    SELECT * FROM retail_sales
    WHERE category = 'Clothing' AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11' AND quantity >= 4;
    ```

- **Total Sales by Category**: Calculate total sales for each category.
    ```sql
    SELECT category, SUM(total_sale) AS net_sale, COUNT(*) AS total_orders
    FROM retail_sales
    GROUP BY category;
    ```

- **Average Age for 'Beauty' Purchasers**: Find the average age of customers who bought items in the 'Beauty' category.
    ```sql
    SELECT ROUND(AVG(age), 2) AS avg_age
    FROM retail_sales
    WHERE category = 'Beauty';
    ```

- **High-Value Transactions**: Transactions where the `total_sale` is over 1000.
    ```sql
    SELECT * FROM retail_sales WHERE total_sale > 1000;
    ```

- **Transactions by Gender and Category**: Total transactions by each gender per category.
    ```sql
    SELECT category, gender, COUNT(*) AS total_trans
    FROM retail_sales
    GROUP BY category, gender
    ORDER BY category;
    ```

- **Monthly Average Sales**: Calculate the average monthly sales and find the best-selling month in each year.
    ```sql
    SELECT year, month, avg_sale
    FROM (
        SELECT EXTRACT(YEAR FROM sale_date) AS year,
               EXTRACT(MONTH FROM sale_date) AS month,
               AVG(total_sale) AS avg_sale,
               RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
        FROM retail_sales
        GROUP BY 1, 2
    ) AS t1
    WHERE rank = 1;
    ```

- **Top 5 Customers by Total Sales**: Identify the top 5 customers based on total sales.
    ```sql
    SELECT customer_id, SUM(total_sale) AS total_sales
    FROM retail_sales
    GROUP BY customer_id
    ORDER BY total_sales DESC
    LIMIT 5;
    ```

- **Unique Customers per Category**: Number of unique customers by category.
    ```sql
    SELECT category, COUNT(DISTINCT customer_id) AS cnt_unique_cs
    FROM retail_sales
    GROUP BY category;
    ```

- **Order Counts by Shift**: Determine the number of orders by shifts (Morning, Afternoon, Evening).
    ```sql
    WITH hourly_sale AS (
        SELECT *,
            CASE
                WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
                WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
                ELSE 'Evening'
            END AS shift
        FROM retail_sales
    )
    SELECT shift, COUNT(*) AS total_orders
    FROM hourly_sale
    GROUP BY shift;
    ```

## Findings

- **Customer Demographics**: Analysis across age groups shows sales are distributed across categories like Clothing and Beauty.
- **High-Value Transactions**: Transactions over 1000 indicate premium purchases.
- **Sales Trends**: Monthly analysis highlights peak and low seasons.
- **Customer Insights**: Identifies top-spending customers and popular categories.

## Reports

- **Sales Summary**: Summary report of total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales patterns across months and shifts.
- **Customer Insights**: Information on top customers and unique customer counts by category.

## Conclusion

This project serves as a comprehensive SQL introduction for data analysts, covering database setup, data cleaning, EDA, and analysis based on business questions. The insights can help businesses understand sales patterns, customer behaviors, and product performance.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts in `database_setup.sql` to create and populate the database.
3. **Run the Queries**: Use the SQL queries in `analysis_queries.sql` to perform analysis.
4. **Explore and Modify**: Feel free to modify queries to explore different aspects or answer additional questions.

---

Happy Analyzing!
