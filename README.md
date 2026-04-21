# Retail-Data-Exploration-Uncovering-Sales-Trends-Customer-Insights-Using-SQL

# 🧠 1. Database Exploration

### 🔹 What this part does:

```sql
SELECT 
    TABLE_CATALOG, 
    TABLE_SCHEMA, 
    TABLE_NAME, 
    TABLE_TYPE  
FROM INFORMATION_SCHEMA.TABLES;
```

👉 This shows:

* All tables in the database
* Their schema (like `gold`)
* Whether they are tables or views

### 💡 Why important:

* First step in any project
* Helps you **understand available data**

---

### 🔹 Column Exploration

```sql
SELECT 
    COLUMN_NAME, 
    DATA_TYPE, 
    IS_NULLABLE, 
    CHARACTER_MAXIMUM_LENGTH
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'dim_customers';
```

👉 This gives:

* Column names
* Data types (INT, VARCHAR, DATE)
* Nullability

### 💡 Purpose:

* Understand table structure before querying

---

# 📊 2. Dimension Exploration

### 🔹 Unique Customer Countries

```sql
SELECT DISTINCT country 
FROM gold.dim_customers
ORDER BY country;
```

👉 Returns:

* List of all countries customers belong to

### 💡 Why:

* Helps understand **geographic distribution**

---

### 🔹 Product Hierarchy

```sql
SELECT DISTINCT 
    category, 
    subcategory, 
    product_name 
FROM gold.dim_products
ORDER BY category, subcategory, product_name;
```

👉 Shows:

* Product structure (Category → Subcategory → Product)

### 💡 Why:

* Understand **product catalog hierarchy**

---

# 📅 3. Date Range Exploration

### 🔹 Sales Date Range

```sql
SELECT 
    MIN(order_date),
    MAX(order_date),
    DATEDIFF(MONTH, MIN(order_date), MAX(order_date))
FROM gold.fact_sales;
```

👉 Gives:

* First order date
* Last order date
* Total duration (in months)

### 💡 Why:

* Understand **how much historical data you have**

---

### 🔹 Customer Age Analysis

```sql
SELECT
    MIN(birthdate),
    DATEDIFF(YEAR, MIN(birthdate), GETDATE()),
    MAX(birthdate),
    DATEDIFF(YEAR, MAX(birthdate), GETDATE())
FROM gold.dim_customers;
```

👉 Finds:

* Oldest customer
* Youngest customer

### 💡 Why:

* Useful for **customer segmentation**

---

# 📈 4. Measures Exploration (Key Metrics)

This is where business insights start 👇

---

### 🔹 Total Sales

```sql
SELECT SUM(sales_amount) FROM gold.fact_sales
```

👉 Total revenue

---

### 🔹 Total Quantity Sold

```sql
SELECT SUM(quantity) FROM gold.fact_sales
```

👉 Total items sold

---

### 🔹 Average Price

```sql
SELECT AVG(price) FROM gold.fact_sales
```

👉 Average selling price

---

### 🔹 Total Orders

```sql
SELECT COUNT(DISTINCT order_number) FROM gold.fact_sales
```

👉 Unique orders (important!)

---

### 🔹 Total Products & Customers

```sql
SELECT COUNT(product_name) FROM gold.dim_products
SELECT COUNT(customer_key) FROM gold.dim_customers;
```

---

### 🔹 Customers Who Actually Purchased

```sql
SELECT COUNT(DISTINCT customer_key) FROM gold.fact_sales;
```

👉 Important insight:

* Not all customers may have placed orders

---

# 📊 5. Combined KPI Report

```sql
SELECT 'Total Sales', SUM(sales_amount)
UNION ALL
SELECT 'Total Quantity', SUM(quantity)
...
```

👉 This creates a **single report like this:**

| measure_name | measure_value |
| ------------ | ------------- |
| Total Sales  | XXXXX         |
| Total Orders | XXXXX         |

### 💡 Why this is powerful:

* Used in dashboards
* Clean KPI summary

---

# 🏆 6. Ranking Analysis (Partially shown)

### 🔹 Purpose:

* Find **Top products / customers**

Uses:

* `RANK()`
* `DENSE_RANK()`
* `ROW_NUMBER()`

👉 Example use case:

* Top 5 products by sales
* Best customers

---

# 🎯 What This Script Really Is

This is called:

### ✅ **Exploratory Data Analysis (EDA) in SQL**

---

# 🚀 Why This Matters (Real World)

This script helps you:

* Understand data structure
* Identify data quality issues
* Build KPIs
* Prepare for dashboards (Power BI / Tableau)

---

# 💬 How to Explain in Interview

You can say:

> "This script performs initial data exploration on a star schema warehouse. It inspects metadata using INFORMATION_SCHEMA, explores dimension tables for categorical insights, analyzes date ranges, and computes key business metrics like total sales, orders, and customers. It also prepares aggregated KPI outputs and supports ranking analysis for identifying top-performing entities."

---

If you want next step, I can:

* Turn this into **advanced SQL interview questions**
* Or help you write **Top 10 business queries (Amazon-level difficulty)**

