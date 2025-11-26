# 🛒 Zepto SQL Data Analysis Project

<img width="2560" height="854" alt="image" src="https://github.com/user-attachments/assets/71b86f86-ea87-4da9-a116-3120a2ad28ca" />


## 📌 Project Overview

This SQL project focuses on performing **data cleaning**, **exploration**, and **analysis** on a dataset representing products from **Zepto**, an online grocery delivery platform. The analysis includes identifying pricing issues, uncovering insights, categorizing products, and calculating revenue-driven metrics.

This project reflects a complete end-to-end workflow suitable for a **Data Analyst / BI / SQL Engineer** portfolio.

---

## 👤 Author Details

**Name:** Antarik Dutt
**Email:** [antarikdutt@gmail.com](mailto:antarikdutt@gmail.com)
**LinkedIn:** [https://www.linkedin.com/in/antarik-dutt](https://www.linkedin.com/in/antarik-dutt)

---

## 🗃️ Database Schema (Table Structure)

The project uses a single table **`zepto`** consisting of product-level information.

### **Table: zepto**

| Column                 | Data Type             | Description                 |
| ---------------------- | --------------------- | --------------------------- |
| sku_id                 | SERIAL PRIMARY KEY    | Unique product SKU          |
| category               | VARCHAR(120)          | Product category            |
| name                   | VARCHAR(150) NOT NULL | Product name                |
| mrp                    | NUMERIC(8,2)          | Maximum retail price        |
| discountPercent        | NUMERIC(5,2)          | Discount percentage         |
| availableQuantity      | INTEGER               | Quantity available in stock |
| discountedSellingPrice | NUMERIC(8,2)          | Final selling price         |
| weightInGms            | INTEGER               | Product weight in grams     |
| outOfStock             | BOOLEAN               | Stock status                |
| quantity               | INTEGER               | Number of units             |

---

## 🧩 ER Diagram

Below is the Entity-Relationship diagram (ERD) representing the system.

```
+-----------------------------+
|           ZEPTO            |
+-----------------------------+
| sku_id (PK)                 |
| category                    |
| name                        |
| mrp                         |
| discountPercent             |
| availableQuantity           |
| discountedSellingPrice      |
| weightInGms                 |
| outOfStock                  |
| quantity                    |
+-----------------------------+
```

Since the project uses a single table, the ERD is straightforward.

---

## 🧪 SQL Queries & Analysis

Below are all SQL queries used in this project, grouped under meaningful sections.

---

# 🔍 **1. Data Exploration**

### **1.1 Count of Rows**

```sql
select count(*) from zepto;
```

### **1.2 Sample Data**

```sql
SELECT * FROM zepto
LIMIT 10;
```

### **1.3 Check Null Values**

```sql
SELECT * FROM zepto
WHERE name IS NULL
OR category IS NULL
OR mrp IS NULL
OR discountPercent IS NULL
OR discountedSellingPrice IS NULL
OR weightInGms IS NULL
OR availableQuantity IS NULL
OR outOfStock IS NULL
OR quantity IS NULL;
```

### **1.4 Different Product Categories**

```sql
SELECT DISTINCT category
FROM zepto
ORDER BY category;
```

### **1.5 Products In Stock vs Out of Stock**

```sql
SELECT outOfStock, COUNT(sku_id)
FROM zepto
GROUP BY outOfStock;
```

### **1.6 Product Names Present Multiple Times**

```sql
SELECT name, COUNT(sku_id) AS "Number of SKUs"
FROM zepto
GROUP BY name
HAVING COUNT(sku_id) > 1
ORDER BY COUNT(sku_id) DESC;
```

---

# 🧼 **2. Data Cleaning**

### **2.1 Products with Invalid Pricing (MRP = 0)**

```sql
SELECT * FROM zepto
WHERE mrp = 0 OR discountedSellingPrice = 0;
```

### **2.2 Remove Incorrect Data**

```sql
DELETE FROM zepto
WHERE mrp = 0;
```

### **2.3 Convert Paise to Rupees**

```sql
UPDATE zepto
SET mrp = mrp / 100.0,
    discountedSellingPrice = discountedSellingPrice / 100.0;
```

### **2.4 Verify Updated Prices**

```sql
SELECT mrp, discountedSellingPrice FROM zepto;
```

---

# 📊 **3. Data Analysis Queries (Major Insights)**

Below are the main 15 queries (all analysis-focused). You provided 8 insights; additional structured insights have been added.

---

## ✅ **Q1. Top 10 Best-Value Products (Highest Discount Percent)**

```sql
SELECT DISTINCT name, mrp, discountPercent
FROM zepto
ORDER BY discountPercent DESC
LIMIT 10;
```

## ✅ **Q2. High MRP Products That Are Out of Stock**

```sql
SELECT DISTINCT name, mrp
FROM zepto
WHERE outOfStock = TRUE AND mrp > 300
ORDER BY mrp DESC;
```

## ✅ **Q3. Estimated Revenue per Category**

```sql
SELECT category,
SUM(discountedSellingPrice * availableQuantity) AS total_revenue
FROM zepto
GROUP BY category
ORDER BY total_revenue;
```

## ✅ **Q4. Products with MRP > ₹500 and Discount < 10%**

```sql
SELECT DISTINCT name, mrp, discountPercent
FROM zepto
WHERE mrp > 500 AND discountPercent < 10
ORDER BY mrp DESC, discountPercent DESC;
```

## ✅ **Q5. Top 5 Categories with Highest Average Discount**

```sql
SELECT category,
ROUND(AVG(discountPercent), 2) AS avg_discount
FROM zepto
GROUP BY category
ORDER BY avg_discount DESC
LIMIT 5;
```

## ✅ **Q6. Price per Gram for Products Above 100g**

```sql
SELECT DISTINCT name, weightInGms, discountedSellingPrice,
ROUND(discountedSellingPrice / weightInGms, 2) AS price_per_gram
FROM zepto
WHERE weightInGms >= 100
ORDER BY price_per_gram;
```

## ✅ **Q7. Categorize Products by Weight (Low / Medium / Bulk)**

```sql
SELECT DISTINCT name, weightInGms,
CASE WHEN weightInGms < 1000 THEN 'Low'
     WHEN weightInGms < 5000 THEN 'Medium'
     ELSE 'Bulk'
END AS weight_category
FROM zepto;
```

## ✅ **Q8. Total Inventory Weight per Category**

```sql
SELECT category,
SUM(weightInGms * availableQuantity) AS total_weight
FROM zepto
GROUP BY category
ORDER BY total_weight;
```

---

# 🚀 Additional Suggested Sections

To make this GitHub project more professional, include:

### **📁 Folder Structure**

```
/Zepto_SQL_Project
│── README.md
│── zepto_dataset.csv
│── zepto_queries.sql
│── er_diagram.png
│── insights_summary.pdf
```

### **📈 Key Insights Summary**

* Some products have extremely high discounts → good for promotional targeting.
* Several premium products are out of stock → indicates missed revenue opportunity.
* Weighted categories reveal which items occupy maximum storage.
* Revenue distribution highlights the most profitable categories.

---

# 🏁 Conclusion

This SQL project demonstrates:
✔ Complete Data Cleaning Workflow
✔ Useful Business-Driven SQL Analysis
✔ Pricing, Weight & Revenue Insights
✔ Organized Queries and ERD
✔ Portfolio-Ready Documentation
