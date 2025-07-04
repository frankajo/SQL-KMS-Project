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

[Download the project dataset here](https://github.com/frankajo/SQL-KMS-Project/blob/main/KMS%20Sql%20Case%20Study.csv)
[Download the project dataset here](https://github.com/frankajo/SQL-KMS-Project/blob/main/Order_Status.csv)

---

### 🧩 Case Scenario I: Sales Performance

#### 1. 🏆 Which product category had the highest sales?

```sql
SELECT TOP 1 Product_Category, SUM(Sales) AS Highest_Sale
FROM [KMS Sql Case Study]
GROUP BY Product_Category
ORDER BY Highest_Sale DESC;
```
![1](https://github.com/user-attachments/assets/c767f5ac-5f07-40da-96f1-3f5faea36e9e)

**Insight**: The "Technology" category recorded the highest total sales.

#### 2. What are the Top 3 and Bottom 3 regions in terms of sales?

**Top 3 Regions**

```sql
SELECT TOP 3 Region, SUM(Sales) AS Total_Sales
FROM [KMS Sql Case Study]
GROUP BY Region
ORDER BY Total_Sales DESC;
```
![top3](https://github.com/user-attachments/assets/342d9913-0ac6-46f7-b154-fca1a34f6065)

**Bottom 3 Regions**

```sql
SELECT TOP 3 Region, SUM(Sales) AS Total_Sales
FROM [KMS Sql Case Study]
GROUP BY Region
ORDER BY Total_Sales ASC;
```
![bottom3](https://github.com/user-attachments/assets/6fb2e5a6-be54-4856-81ff-7b92e0e9f689)

**Insight**: Western and Eastern regions dominate sales. Northern region underperforms.

#### 3. What were the total sales of appliances in Ontario?

```sql
SELECT SUM(Sales) AS Ontario_Appliance_Sales
FROM [KMS Sql Case Study]
WHERE Region = 'Ontario' AND Product_Sub_Category = 'Appliances';
```
![3](https://github.com/user-attachments/assets/17de9f79-52e2-46bd-9b8b-1cfbba2e8539)

**Insight**: Appliances sales in Ontario amounted to a significant total but were not the top-performing product.

#### 4.  Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers 

```sql
Select Top 10 Customer_Name, Region, Order_Quantity, Discount,
Sum(Sales) as [Total Sales],
Sum(Profit) as [Total Profit]
from [KMS Sql Case Study]
group by Customer_Name, Region, Order_Quantity, Discount
Order by [Total Sales] Asc;
```
![4](https://github.com/user-attachments/assets/fa11e60d-0171-420d-a6fa-1ba31a464072)

**Insight**: In consideration with the top 10 customers, Discount or Region is not the the reason for their poor patronage. I advise the management of KMS to..
* Send short surveys to learn why they’re not buying more.
* Offer exclusive discounts or loyalty points to motivate purchases.
* Offer free shipping or cashback for second-time purchases.
* Introduce a “Buy X, Get Y Free” strategy or referral bonuses.

#### 5. Which shipping method incurred the most cost?

```sql
SELECT TOP 1 Ship_Mode, SUM(Shipping_Cost) AS Max_Shipping_Cost
FROM [KMS Sql Case Study]
GROUP BY Ship_Mode
ORDER BY Max_Shipping_Cost DESC;
```
![5](https://github.com/user-attachments/assets/a70e842b-be65-42bb-9607-90069737b14f)

**Insight**: "Delivery Truck" incure the most shipping cost, confirming management concerns.

---

### 👤 Case Scenario II: Customer Insights

#### 6. Who are the most valuable customers, and what products do they purchase?

```sql
SELECT TOP 10 Customer_Name, Product_Name,
       SUM(Sales) AS Total_Sales, SUM(Profit) AS Total_Profit
FROM [KMS Sql Case Study]
GROUP BY Customer_Name, Product_Name
ORDER BY Total_Profit DESC;
```
![6](https://github.com/user-attachments/assets/51eaef73-bd51-44e7-9270-f1e1c401d3f8)

**Insight**: A small group of customers contribute significantly to overall profit.

#### 7. Which Small Business customer had the highest sales?

```sql
SELECT TOP 1 Customer_Name, SUM(Sales) AS Highest_Sales
FROM [KMS Sql Case Study]
WHERE Customer_Segment = 'Small Business'
GROUP BY Customer_Name
ORDER BY Highest_Sales DESC;
```
![7](https://github.com/user-attachments/assets/83a357e1-8109-4d16-a356-8075f5271372)

**Insight**: Identifies the top-performing small business for potential upsell or retention efforts.

#### 8. 🗓️ Which Corporate Customer placed the most orders (2009–2012)?

```sql
SELECT TOP 1 Customer_Name, COUNT(Order_ID) AS Number_Of_Orders
FROM [KMS Sql Case Study]
WHERE Customer_Segment = 'Corporate'
  AND Order_Date BETWEEN '2009-01-01' AND '2012-12-31'
GROUP BY Customer_Name
ORDER BY Number_Of_Orders DESC;
```
![8](https://github.com/user-attachments/assets/32f4e1f7-afbf-4422-839f-08e58360ff75)


#### 9. Which Consumer customer was the most profitable?

```sql
SELECT TOP 1 Customer_Name, SUM(Profit) AS Most_Profit
FROM [KMS Sql Case Study]
WHERE Customer_Segment = 'Consumer'
GROUP BY Customer_Name
ORDER BY Most_Profit DESC;
```
![9](https://github.com/user-attachments/assets/cace091d-9415-4f59-8cf2-e7aa9a2340b3)

**Insight**: Single out the most profitable consumer for potential case study or feedback.

#### 10. Which customers returned items, and what segment do they belong to?

```sql
SELECT DISTINCT Ods.Order_ID, Kms.Customer_Name,
       Kms.Customer_Segment, Ods.Status
FROM [KMS Sql Case Study] AS Kms
JOIN Order_Status AS Ods ON Kms.Order_ID = Ods.Order_ID;
```
![10](https://github.com/user-attachments/assets/e4bd4902-cda9-49ae-a3da-125782801c02)

**Insight**: Return behavior is visible across segments; About 573 customers returned products.

#### 11. Did KMS appropriately spend shipping costs by Order Priority?

```sql
SELECT Order_Priority, Ship_Mode,
       COUNT(Order_ID) AS Total_Deliveries,
       ROUND(SUM(Sales - Profit), 2) AS Estimated_Shipping_Cost,
       AVG(DATEDIFF(DAY, Order_Date, Ship_Date)) AS Avg_Shipping_Days
FROM [KMS Sql Case Study]
GROUP BY Order_Priority, Ship_Mode;
```
![11](https://github.com/user-attachments/assets/0c9bc94e-482c-4c7e-9280-33adf322868d)

**Insight**: Confirms mismatch between urgency (priority) and cost/time spent on shipping. Opportunity for optimization.

---

## 📌 Recommendations

* Focus on top-performing regions and profitable customer segments
* Offer incentives to bottom 10 customers to improve engagement
* Reevaluate the use of Express Air for low-priority orders
* Expand high-profit product lines into underperforming regions
* Monitor and reduce return rates through customer feedback

---

### 🛠 Tools & Technologies

* SQL Server Management Studio (SSMS)
* Excel (Data preparation)
* GitHub (Documentation)

---


### ✅ Conclusion

This case study provided a data-driven approach to uncovering key opportunities and challenges within KMS. The findings offer practical ways to improve revenue, optimize shipping operations, and strengthen customer relationships using SQL-powered analysis.

---
