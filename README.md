# 📊 Kultra Mega Stores (KMS) SQL Data Analysis Case Study
## Overview
This case study analyzes Kultra Mega Stores’ (KMS) sales, customer segments, shipping methods, and profitability using SQL.
The goal was to answer key business questions and provide actionable recommendations for revenue growth and operational efficiency.

## 1️⃣ Highest Sales by Product Category
```
SELECT TOP 1 Product_Category, SUM(Sales) AS TotalSales
FROM [KMS data]
GROUP BY Product_Category
ORDER BY TotalSales DESC
```
**Result:**

*Technology* – $5,984,249

## 2️⃣ Top 3 & Bottom 3 Regions by Sales
```
-- Top 3
SELECT TOP 3 Region, SUM(Sales) AS TotalSales
FROM [KMS data]
GROUP BY Region
ORDER BY TotalSales DESC

-- Bottom 3
SELECT TOP 3 Region, SUM(Sales) AS TotalSales
FROM [KMS data]
GROUP BY Region
ORDER BY TotalSales ASC
```
**Results:**

Top 3: West ($3.60M), Ontario ($3.06M), Prairie ($2.84M)

Bottom 3: Nunavut ($116K), Northwest Territories ($801K), Yukon ($976K)

## 3️⃣ Appliance Sales in Ontario
```
SELECT Region, SUM(Sales) AS ApplianceSales_Ontario
FROM [KMS data]
WHERE Product_Sub_Category = 'Appliances' AND Region = 'Ontario'
GROUP BY Region
```
**Result:**
Ontario – $202,346.84

## 4️⃣ Bottom 10 Customers – Revenue Growth Strategy
```
SELECT TOP 10 Order_ID, Customer_Name, SUM(Sales) AS TotalSales
FROM [KMS data]
GROUP BY Order_ID, Customer_Name
ORDER BY TotalSales ASC
```
**Lowest Sales Customers:**
Ken Dana ($3.20), Adam Hart ($3.41), Andy Reiter ($3.42) … up to Ruben Dartt ($6.93)

**Recommendations:**

Offer loyalty discounts & targeted promotions

Upsell/cross-sell frequently purchased items

Provide better customer service follow-up

Engage via email marketing

## 5️⃣ Shipping Cost by Mode
```
SELECT TOP 1 Ship_Mode, SUM(Shipping_Cost) AS TotalShippingCost
FROM [KMS data]
GROUP BY Ship_Mode
ORDER BY TotalShippingCost DESC
```
**Result:**
Delivery Truck – $51,971.94

## 6️⃣ Most Valuable Customers & Purchase Patterns
```
SELECT TOP 10 Row_ID, Order_ID, Customer_Name, product_category, SUM(Profit) AS TotalProfit
FROM [KMS data]
GROUP BY Row_ID, Order_ID, Customer_Name, product_category
ORDER BY TotalProfit DESC
```
**Top Customers (By Profit):**

Emily Phan – Technology ($27,220.69)

Andy Reiter – Technology ($14,440.39)

Deborah Brumfield – Technology ($13,340.26)

**Insight:** High-value customers mostly purchase Technology products.

## 7️⃣ Small Business Customers with Highest Sales
```
SELECT TOP 5 Order_ID, Customer_Name, Customer_Segment, MAX(Sales) AS SALES
FROM [KMS data]
WHERE Customer_Segment = 'Small Business'
GROUP BY Order_ID, Customer_Name, Customer_Segment
ORDER BY SALES DESC
```
**Top Small Business:** Dennis Kane – $33,367.85

## 8️⃣ Corporate Customers with Most Orders (2009–2012)
```
SELECT TOP 10 Customer_Name, COUNT(Order_ID) AS OrderCount
FROM [KMS data]
WHERE Customer_Segment = 'Corporate'
  AND Ship_Date BETWEEN '2009-01-01' AND '2012-12-31'
GROUP BY Customer_Name
ORDER BY OrderCount DESC
```
**Leader:** Adam Hart – 27 orders

## 9️⃣ Most Profitable Consumer Customer
```
SELECT Top 1 Order_ID, Customer_Name, MAX(Profit) AS Max_Profit
FROM [KMS data]
WHERE Customer_Segment = 'Consumer'
GROUP BY Order_ID, Customer_Name
ORDER BY Max_Profit DESC
```
**Result:** Emily Phan – $27,220.69

## 🔟 Product Returns Analysis
```
-- Count of returns by segment
SELECT Customer_Segment, COUNT(Order_ID) AS Returned_Orders
FROM [KMS data]
JOIN Order_Status ON [KMS data].Order_ID = Order_Status.Order_ID
WHERE Order_Status.Status = 'Returned'
GROUP BY Customer_Segment
ORDER BY Returned_Orders DESC
```
**Returned Orders by Segment:**

Corporate – 334

Home Office – 201

Small Business – 190

Consumer – 147

**Total Returned Orders:** 872

## 1️⃣1️⃣ Shipping Cost vs Order Priority
```
Edit
SELECT Ship_Mode, Order_Priority, COUNT(*) AS OrderCount, SUM(Shipping_Cost) AS TotalShippingCost
FROM [KMS data]
GROUP BY Ship_Mode, Order_Priority
ORDER BY Order_Priority, Ship_Mode
```
**Findings:**

Critical orders often shipped via Delivery Truck, costing more than Express Air in total.

Some low and medium priority orders also used expensive methods.

Assumed cost ranking (Truck < Air) doesn’t hold true in all cases — likely due to package size/weight.

## Recommendations:

Perform cost-per-unit analysis for each shipping method.

Align shipping method with order priority.

Revisit logistics contracts.

##💡 Key Takeaways
Technology is the leading revenue driver — focus on upselling in this category.

Underperforming regions need targeted marketing.

Shipping cost allocation should be data-driven, not assumption-based.

Retention strategies for low-value customers can boost long-term sales.

Product returns warrant deeper quality control reviews.

