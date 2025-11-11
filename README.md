# 📊 Sales Analysis Dashboard

## 📝 Project Overview
Welcome to the **Sales Analysis Dashboard** project!  
This interactive Tableau dashboard provides comprehensive insights into **sales performance and customer behavior**. Users can explore **Total Sales**, **Profit**, and **Quantity** metrics with **year-on-year comparisons**, analyze trends by **product subcategory**, and uncover **customer insights** based on order volumes.  

With flexible filters for **year**, **category**, **subcategory**, **region**, and more, the dashboard empowers businesses to make **data-driven decisions** and identify opportunities for growth.

**Dashboard Previews:**  
- 🧾 *Sales Dashboard*  
- 👥 *Customer Dashboard*

---<img width="1366" height="768" alt="Screenshot Sales Dashboard" src="https://github.com/user-attachments/assets/4d934fd3-c070-4507-bff4-475623b973cd" />
---<img width="1366" height="768" alt="Screenshot Customer dashboard" src="https://github.com/user-attachments/assets/fbd93cb5-84f0-4cf5-942a-f4e3a467cf08" />


## 🗂️ Data Source
The dataset used in this project, `Sales_data.csv`, contains detailed sales transactions across multiple regions and product categories, spanning **January to December**.

---

## 🧰 Tools Used
- **MySQL** – Used to query and extract data from the database. SQL queries retrieved specific subsets of data required for analysis.  
- **Microsoft Excel** – Utilized for **data cleaning**, quick validations, and exploratory checks before importing into Tableau.  
- **Tableau** – Designed and developed **interactive dashboards** visualizing sales metrics, trends, and KPIs. Leveraged calculated fields and Tableau’s drag-and-drop interface to build dynamic visualizations.  

---

## 🧹 Data Cleaning & Preparation
The dataset includes metrics such as **Total Sales**, **Profit**, **Quantity**, **Total Customers**, and **Sales per Customer** for the entire year.

### 🔧 Data Cleaning Steps
1. **Handled Missing Values** – Removed or imputed missing data using mean/median for numerical columns.  
2. **Data Type Conversion** – Converted date fields to `datetime` and ensured type consistency across variables.  
3. **Removed Duplicates** – Eliminated duplicate records to preserve data integrity.  
4. **Standardized Formats** – Unified product names, region names, and customer categories.  
5. **Addressed Outliers** – Applied winsorization to reduce the impact of extreme values.  
6. **Data Transformation** – Aggregated data monthly and created derived variables for deeper analysis.  
7. **Ensured Data Integrity** – Cross-validated related columns and corrected inconsistencies.  
8. **Validated Data** – Compared against external references and conducted logic checks.  

### ✅ Result
A **clean, reliable, and analysis-ready dataset** ensuring consistency, accuracy, and trustworthiness of all insights derived from it.

---

## 🔍 Exploratory Data Analysis (EDA) Questions
The following questions guided the exploratory analysis:

- How do **Total Sales**, **Total Profit**, and **Total Quantity** vary over time?  
- How does performance compare **year-over-year**?  
- Which **product subcategories** contribute most to sales and profit?  
- How many customers exist per month, and what’s their **average spending**?  
- How do **customer segments** differ in terms of profitability?  
- How does sales performance vary across **regions, states, and cities**?  
- Which regions achieve the **highest sales and profitability**?  
- What are the **top-selling categories and subcategories**?  
- Are there notable **trends or correlations** among sales metrics and other variables?

---

## 💻 Data Analysis with SQL

### Example SQL Queries
```sql
-- Extracting sales data for analysis
SELECT *
FROM sales_data
WHERE date BETWEEN '2021-01-01' AND '2021-12-31';

-- Aggregating sales data by month
SELECT MONTH(date) AS month, 
       SUM(total_sales) AS total_sales,
       SUM(total_profit) AS total_profit
FROM sales_data
GROUP BY MONTH(date)
ORDER BY month;

-- Filtering data by product category and subcategory
SELECT *
FROM sales_data
WHERE category = 'Furniture' AND subcategory = 'Chairs';

-- Joining sales and customer tables
SELECT s.*, c.customer_name
FROM sales_data s
INNER JOIN customers c ON s.customer_id = c.customer_id;

-- Calculating year-on-year growth in total sales
SELECT YEAR(date) AS year, 
       SUM(total_sales) AS total_sales,
       (SUM(total_sales) - LAG(SUM(total_sales), 1) OVER (ORDER BY YEAR(date))) 
         / LAG(SUM(total_sales), 1) OVER (ORDER BY YEAR(date)) AS yoy_growth
FROM sales_data
GROUP BY YEAR(date)
ORDER BY year;

-- Identifying top-selling products
SELECT product_name, SUM(quantity) AS total_quantity
FROM sales_data
GROUP BY product_name
ORDER BY total_quantity DESC
LIMIT 10;

-- Analyzing customer behavior
SELECT customer_id, COUNT(DISTINCT order_id) AS num_orders
FROM sales_data
GROUP BY customer_id
ORDER BY num_orders DESC;
