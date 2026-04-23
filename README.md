# 📊 Exploratory Data Analysis of Retail Sales Using SQL (KPIs & Ranking Insights)

---

## 1️⃣ Executive Summary

This project performs a comprehensive **Exploratory Data Analysis (EDA)** on a retail data warehouse using SQL. The analysis focuses on understanding data structure, generating key business metrics, and identifying top and bottom performers across products and customers.

By leveraging SQL techniques such as aggregation, joins, and window functions, the project transforms raw transactional data into actionable business insights that support decision-making in sales, marketing, and product strategy.

---

## 2️⃣ Business Problem

Retail businesses often struggle to:

* Understand overall sales performance
* Identify high-value customers and top-performing products
* Track key business KPIs efficiently
* Detect underperforming products or low-engagement customers

Without proper analysis, this leads to missed revenue opportunities and inefficient decision-making.

This project aims to solve these challenges using structured SQL-based data exploration.

---

## 3️⃣ Methodology

The analysis follows a structured EDA approach:

### 🔹 Database Exploration

* Used `INFORMATION_SCHEMA` to inspect tables and columns
* Understood schema design (fact & dimension tables)

### 🔹 Dimension Analysis

* Explored customer demographics (e.g., countries)
* Analyzed product hierarchy (category, subcategory)

### 🔹 Date Range Analysis

* Identified data coverage (first and last order dates)
* Analyzed customer age distribution

### 🔹 KPI & Metrics Calculation

* Total Sales
* Total Quantity Sold
* Average Price
* Total Orders (distinct)
* Total Customers & Active Customers

### 🔹 Reporting Layer

* Combined KPIs into a unified report using `UNION ALL`

### 🔹 Ranking Analysis

* Identified:

  * Top 5 products by revenue
  * Bottom 5 products
  * Top 10 customers
  * Least active customers
* Used:

  * `GROUP BY`
  * `ORDER BY`
  * Window functions (`RANK()`)

---

## 4️⃣ Skills

This project demonstrates the following skills:

* SQL Data Analysis
* Exploratory Data Analysis (EDA)
* Data Warehousing Concepts (Fact & Dimension Tables)
* Aggregations (SUM, AVG, COUNT)
* Joins (Fact-Dimension Relationships)
* Window Functions (RANK)
* KPI Design & Reporting
* Business Insight Generation

---

## 5️⃣ Results & Business Recommendation

### 📊 Key Insights:

* Identified top-performing products contributing the highest revenue
* Discovered low-performing products that may require review or discontinuation
* Recognized high-value customers driving significant revenue
* Measured overall business performance using KPIs

### 💡 Business Recommendations:

* Focus marketing efforts on high-value customer segments
* Promote top-performing products to maximize revenue
* Re-evaluate or optimize underperforming products
* Use KPI dashboards for continuous monitoring

---

## 6️⃣ Next Steps

To enhance this project further:

* 📈 Build a Power BI / Tableau dashboard for visualization
* ⏳ Add time-series analysis (monthly/quarterly trends)
* 👥 Perform advanced customer segmentation (RFM analysis)
* 🤖 Integrate predictive analytics (sales forecasting)
* 🧩 Automate data pipeline for real-time reporting

---

## 📌 Conclusion

This project showcases how SQL can be effectively used to explore data, generate KPIs, and derive meaningful business insights from a retail dataset. It highlights both technical expertise and analytical thinking required for real-world data analyst roles.

---

---

# 🧠 1. Database Exploration

### 🔹 Code:

```sql
SELECT TABLE_CATALOG,
        TABLE_SCHEMA,
        TABLE_NAME,
        TABLE_TYPE  
FROM INFORMATION_SCHEMA.TABLES;
```
<img width="639" height="511" alt="image" src="https://github.com/user-attachments/assets/36ec90f9-9b7b-4428-bb42-bfae1360e997" />


👉 **What it does:**

* Lists all tables in the database

👉 **Why:**

* First step in EDA → understand what data is available

---

### 🔹 Column Metadata:

```sql
SELECT COLUMN_NAME,
       DATA_TYPE,
       IS_NULLABLE,
       CHARACTER_MAXIMUM_LENGTH
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'dim_customers';
```
<img width="678" height="476" alt="image" src="https://github.com/user-attachments/assets/42caeee3-0e15-482e-b734-265eca480217" />

👉 **What it does:**

* Shows structure of `dim_customers`

👉 **Why:**

* Helps you understand:

  * Data types
  * Missing values possibility

---

# 📊 2. Dimension Exploration

### 🔹 Unique Countries:

```sql
SELECT DISTINCT country 
FROM gold.dim_customers
ORDER BY country;
```
<img width="799" height="348" alt="image" src="https://github.com/user-attachments/assets/f11a0349-3b03-4593-9cce-6b0347acb7dd" />

👉 **Purpose:**

* Identify where customers are from

👉 **Insight Use:**

* Geographic segmentation

---

### 🔹 Product Hierarchy:

```sql
SELECT DISTINCT category, subcategory, product_name 
FROM gold.dim_products
ORDER BY category, subcategory, product_name;
```
<img width="776" height="588" alt="image" src="https://github.com/user-attachments/assets/38ceacc0-04dc-40e5-93be-7d3da098010c" />

👉 **Purpose:**

* Understand product structure

👉 **Insight Use:**

* Category-level sales analysis

---

# 📅 3. Date Range Exploration

### 🔹 Sales Time Range:

```sql
SELECT 
    MIN(order_date),
    MAX(order_date),
    DATEDIFF(MONTH, MIN(order_date), MAX(order_date))
FROM gold.fact_sales;
```
<img width="846" height="320" alt="image" src="https://github.com/user-attachments/assets/4f1a4033-7d63-45b9-afce-b1ebbc449361" />

👉 **What it does:**

* Finds:

  * First order date
  * Last order date
  * Duration in months

👉 **Why important:**

* Understand how much historical data exists

---

### 🔹 Customer Age Analysis:

```sql
SELECT
    MIN(birthdate),
    DATEDIFF(YEAR, MIN(birthdate), GETDATE()),
    MAX(birthdate),
    DATEDIFF(YEAR, MAX(birthdate), GETDATE())
FROM gold.dim_customers;
```
<img width="754" height="329" alt="image" src="https://github.com/user-attachments/assets/c592463e-0e47-4d4a-a2f9-47573d19c9b9" />

👉 **What it does:**

* Finds:

  * Oldest customer
  * Youngest customer

👉 **Use:**

* Demographic analysis

---

# 📈 4. Measures Exploration (KPIs)

This section calculates **business metrics** 👇

---

### 🔹 Total Sales

```sql
SELECT SUM(sales_amount) FROM gold.fact_sales
```

👉 Total revenue generated

---

### 🔹 Total Quantity

```sql
SELECT SUM(quantity) FROM gold.fact_sales
```

👉 Total units sold

---

### 🔹 Average Price

```sql
SELECT AVG(price) FROM gold.fact_sales
```

👉 Pricing insight

---

### 🔹 Orders Count

```sql
SELECT COUNT(order_number)
SELECT COUNT(DISTINCT order_number)
```

👉 Key point:

* `COUNT` → counts rows
* `DISTINCT` → counts unique orders ✅

---

### 🔹 Customers & Products

```sql
SELECT COUNT(product_name) FROM gold.dim_products
SELECT COUNT(customer_key) FROM gold.dim_customers
```

---

### 🔹 Active Customers

```sql
SELECT COUNT(DISTINCT customer_key) FROM gold.fact_sales;
```

👉 Only customers who made purchases

---

# 📊 5. KPI Report (Very Important)


<img width="947" height="520" alt="image" src="https://github.com/user-attachments/assets/c76f6332-53ae-4f3c-9559-eacb18aabeb2" />

👉 **What it does:**

* Combines multiple metrics into one table

👉 Output example:

| measure_name | value |
| ------------ | ----- |
| Total Sales  | ...   |
| Total Orders | ...   |

👉 **Why powerful:**

* Used directly in dashboards

---

# 🏆 6. Ranking Analysis (Advanced EDA)

---

## 🔹 Top 5 Products (Simple)

```sql
SELECT TOP 5 p.product_name, SUM(f.sales_amount)
FROM fact_sales f
JOIN dim_products p
GROUP BY p.product_name
ORDER BY total_revenue DESC;
```
<img width="661" height="422" alt="image" src="https://github.com/user-attachments/assets/f71f24e0-477a-49db-b248-e02e3782ca58" />

👉 Finds best-selling products

---

## 🔹 Top 5 Products (Window Function)

<img width="826" height="505" alt="image" src="https://github.com/user-attachments/assets/317ab28e-93bd-4862-a0c3-c8f59c20c15a" />


👉 **Why better:**

* More flexible (handles ties)

---

## 🔹 Bottom 5 Products

<img width="710" height="428" alt="image" src="https://github.com/user-attachments/assets/8d69dfbf-9123-41b0-bff8-37091cd5242c" />

👉 Finds worst performers

---

## 🔹 Top 10 Customers

<img width="743" height="599" alt="image" src="https://github.com/user-attachments/assets/62452f02-adbe-4039-874a-7c92176c8589" />

👉 Identifies high-value customers

---

## 🔹 Lowest 3 Customers (by orders)

<img width="647" height="520" alt="image" src="https://github.com/user-attachments/assets/ddf09a1d-4595-4a5c-9f62-efea611c1cd3" />


👉 Finds least engaged customers

---

# 🎯 What This Script Actually Is

👉 This is:

### ✅ **Full SQL-Based Exploratory Data Analysis (EDA)**

---

# 🚀 What Skills This Demonstrates

This single script shows:

✔ Data exploration
✔ Schema understanding
✔ Aggregation (SUM, AVG, COUNT)
✔ KPI generation
✔ Joins (fact + dimension)
✔ Window functions (RANK)
✔ Business thinking

---

