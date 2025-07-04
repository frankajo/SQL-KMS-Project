# DSA Project Documentation
My name is `Ngbede Frank Ajo`. This is the first test of my ability as a Data Analyst. I want to say a big thank you to `The Incubator Hub` and 
the entire `DSA Team` for this life changing experience, and my wonderful, impactful and friendly Facilitators
`(Mr. Temidayo DeeTee, Mr. Femi Ayodele and Mr. Muhsin Hameed)` for their wonderful impact in my journey.

My special thank goes to my Father in The Lord, `Daddy G.O` who made this program possible for me to enjoy free of charge. 
May Almighty continue to bless protect you. We love you…

---

## KMS SQL Case Study: Sales, Customers & Shipping Analysis

---

### 🧾 Project Overview

KMS is a retail company aiming to better understand the performance of its sales, customer segments, product categories, and shipping costs. Using SQL, this project analyzes the sales dataset to extract actionable business insights that support decision-making across customer targeting, regional expansion, and cost management.

---

### Problem Statement

KMS management lacks visibility into the key drivers of revenue, profitability, and shipping costs. Strategic decisions are currently based on assumptions rather than data-driven insights.

---

### 🎯 Project Objectives

* Discover the most and least profitable customers
* Analyze shipping methods and costs relative to order priorities
* Provide data-backed recommendations to increase revenue and optimize costs

---

### 📊 Dataset Summary

The dataset includes retail transactions from 2009 to 2012 with fields such as:

* `Order_ID`, `Customer_Name`, `Customer_Segment`
* `Product_Category`, `Product_Sub_Category`, `Product_Name`
* `Region`, `Sales`, `Profit`, `Shipping_Cost`, `Ship_Mode`
* `Order_Date`, `Ship_Date`, `Order_Priority`
* * `Status_Status`

Preprocessing was done in Excel to properlly reformat the date column, the prices columns into currency and standardize headers. 
SQL Server was used for querying.

---

## 🧩 Case Scenario I: Sales Performance

### 1. 🏆 Which product category had the highest sales?

```sql
SELECT TOP 1 Product_Category, SUM(Sales) AS Highest_Sale
FROM [KMS Sql Case Study]
GROUP BY Product_Category
ORDER BY Highest_Sale DESC;
```
![1](https://github.com/user-attachments/assets/6a96f29d-ccb2-4939-b8e2-80c15c552cf4)


**Insight**: The "Technology" category recorded the highest total sales.

### 2. 🌍 What are the Top 3 and Bottom 3 regions in terms of sales?

**Top 3 Regions**

```sql
SELECT TOP 3 Region, SUM(Sales) AS Total_Sales
FROM [KMS Sql Case Study]
GROUP BY Region
ORDER BY Total_Sales DESC;
```

**Bottom 3 Regions**

```sql
SELECT TOP 3 Region, SUM(Sales) AS Total_Sales
FROM [KMS Sql Case Study]
GROUP BY Region
ORDER BY Total_Sales ASC;
```
![2](https://github.com/user-attachments/assets/4c5b5924-0de9-4d63-9d00-66d5e73ab052)

**Insight**: Western and Eastern regions dominate sales. Northern region underperforms.

### 3. What were the total sales of appliances in Ontario?

```sql
SELECT SUM(Sales) AS Ontario_Appliance_Sales
FROM [KMS Sql Case Study]
WHERE Region = 'Ontario' AND Product_Sub_Category = 'Appliances';
```
![3](https://github.com/user-attachments/assets/4fed9b17-82b3-4b6e-9b48-f57b3f82b1ec)

**Insight**: Appliances sales in Ontario amounted to a significant total but were not the top-performing product.

### 4. 📉 Revenue from the Bottom 10 Customers

```sql
SELECT TOP 10 Customer_Name, SUM(Sales) AS Total_Revenue
FROM [KMS Sql Case Study]
GROUP BY Customer_Name
ORDER BY Total_Revenue ASC;
```
![4](https://github.com/user-attachments/assets/192dd443-61f4-4733-9d7c-6892717e409e)

**Insight**: The bottom 10 customers contribute minimally to revenue and may benefit from promotional targeting.

### 5. 🚚 Which shipping method incurred the most cost?

```sql
SELECT TOP 1 Ship_Mode, SUM(Shipping_Cost) AS Max_Shipping_Cost
FROM [KMS Sql Case Study]
GROUP BY Ship_Mode
ORDER BY Max_Shipping_Cost DESC;
```

**Insight**: "Express Air" had the highest shipping cost, confirming management concerns.

---

## 👤 Case Scenario II: Customer Insights

### 6. 💎 Who are the most valuable customers, and what products do they purchase?

```sql
SELECT TOP 10 Customer_Name, Product_Name,
       SUM(Sales) AS Total_Sales, SUM(Profit) AS Total_Profit
FROM [KMS Sql Case Study]
GROUP BY Customer_Name, Product_Name
ORDER BY Total_Profit DESC;
```

**Insight**: A small group of customers contribute significantly to overall profit.

### 7. 🧾 Which Small Business customer had the highest sales?

```sql
SELECT TOP 1 Customer_Name, SUM(Sales) AS Highest_Sales
FROM [KMS Sql Case Study]
WHERE Customer_Segment = 'Small Business'
GROUP BY Customer_Name
ORDER BY Highest_Sales DESC;
```

**Insight**: Identifies the top-performing small business for potential upsell or retention efforts.

### 8. 🗓️ Which Corporate Customer placed the most orders (2009–2012)?

```sql
SELECT TOP 1 Customer_Name, COUNT(Order_ID) AS Number_Of_Orders
FROM [KMS Sql Case Study]
WHERE Customer_Segment = 'Corporate'
  AND Order_Date BETWEEN '2009-01-01' AND '2012-12-31'
GROUP BY Customer_Name
ORDER BY Number_Of_Orders DESC;
```

**Insight**: Shows repeat Corporate buyers and helps segment high-frequency clients.

### 9. 💰 Which Consumer customer was the most profitable?

```sql
SELECT TOP 1 Customer_Name, SUM(Profit) AS Most_Profit
FROM [KMS Sql Case Study]
WHERE Customer_Segment = 'Consumer'
GROUP BY Customer_Name
ORDER BY Most_Profit DESC;
```

**Insight**: Single out the most profitable consumer for potential case study or feedback.

### 10. 🔁 Which customers returned items, and what segment do they belong to?

```sql
SELECT DISTINCT Ods.Order_ID, Kms.Customer_Name,
       Kms.Customer_Segment, Ods.Status
FROM [KMS Sql Case Study] AS Kms
JOIN Order_Status AS Ods ON Kms.Order_ID = Ods.Order_ID;
```

**Insight**: Return behavior is visible across segments; helps with quality checks.

### 11. 🚛 Did KMS appropriately spend shipping costs by Order Priority?

```sql
SELECT Order_Priority, Ship_Mode,
       COUNT(Order_ID) AS Total_Deliveries,
       ROUND(SUM(Sales - Profit), 2) AS Estimated_Shipping_Cost,
       AVG(DATEDIFF(DAY, Order_Date, Ship_Date)) AS Avg_Shipping_Days
FROM [KMS Sql Case Study]
GROUP BY Order_Priority, Ship_Mode;
```

**Insight**: Confirms mismatch between urgency (priority) and cost/time spent on shipping. Opportunity for optimization.

---

## 📌 Recommendations

* Focus on top-performing regions and profitable customer segments
* Offer incentives to bottom 10 customers to improve engagement
* Reevaluate the use of Express Air for low-priority orders
* Expand high-profit product lines into underperforming regions
* Monitor and reduce return rates through customer feedback

---

## 🛠 Tools & Technologies

* SQL Server Management Studio (SSMS)
* Excel (Data preparation)
* Power BI (Optional visualization)
* GitHub (Documentation)

---

## 📁 Project Structure

| File/Folder | Description                     |
| ----------- | ------------------------------- |
| `queries/`  | SQL queries grouped by scenario |
| `data/`     | Cleaned dataset (CSV or Excel)  |
| `visuals/`  | Power BI or Excel screenshots   |
| `README.md` | Project documentation           |

---

## ✅ Conclusion

This case study provided a data-driven approach to uncovering key opportunities and challenges within KMS. The findings offer practical ways to improve revenue, optimize shipping operations, and strengthen customer relationships using SQL-powered analysis.

---
