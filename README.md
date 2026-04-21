
---

# 🧠 1. Database Exploration

### 🔹 Code:

```sql
SELECT TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME, TABLE_TYPE  
FROM INFORMATION_SCHEMA.TABLES;
```

👉 **What it does:**

* Lists all tables in the database

👉 **Why:**

* First step in EDA → understand what data is available

---

### 🔹 Column Metadata:

```sql
SELECT COLUMN_NAME, DATA_TYPE, IS_NULLABLE, CHARACTER_MAXIMUM_LENGTH
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'dim_customers';
```

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

```sql
SELECT 'Total Sales', SUM(sales_amount)
UNION ALL
SELECT 'Total Quantity', SUM(quantity)
...
```

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

👉 Finds best-selling products

---

## 🔹 Top 5 Products (Window Function)

```sql
RANK() OVER (ORDER BY SUM(sales_amount) DESC)
```

👉 **Why better:**

* More flexible (handles ties)

---

## 🔹 Bottom 5 Products

```sql
ORDER BY total_revenue
```

👉 Finds worst performers

---

## 🔹 Top 10 Customers

```sql
SUM(sales_amount)
```

👉 Identifies high-value customers

---

## 🔹 Lowest 3 Customers (by orders)

```sql
COUNT(DISTINCT order_number)
ORDER BY total_orders
```

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

# 💬 Interview Explanation (Use This!)

> "This SQL script performs end-to-end exploratory data analysis on a retail data warehouse. It begins with schema exploration using INFORMATION_SCHEMA, analyzes dimensions like customers and products, evaluates date ranges, and computes key business KPIs such as total sales and orders. It also includes ranking analysis to identify top and bottom performing products and customers using aggregation and window functions."

---

# 🧠 Simple Summary

👉 In one line:

> This code **explores data, calculates KPIs, and identifies top/bottom performers in a retail dataset using SQL**

---

If you want next step, I can:

* Turn this into **real business insights (like storytelling for interviews)**
* Or give **advanced SQL questions based on this script (Amazon-level)** 🚀

