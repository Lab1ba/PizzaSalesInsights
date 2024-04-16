# Pizza Sales Insights

### Dashboard Link : https://app.powerbi.com/groups/44299d38-d5e6-46b4-84ab-21e4c742865f/reports/20897431-2dc4-4dd9-a81b-a5b6767c7fa1/ReportSection?experience=power-bi

PROBLEM STATEMENT

- KPI'S REQUIREMENT:

We need to analyze key indicators for our pizza sales data to gain insights into our business performance. Specially, we want to calculate the following metrics:

1. Total Revenue: The sum of the total price of all pizza orders.

2. Average Order Value: The average amount spent per order, calculated by dividing the total revenue by the total number of orders.
3. Total Pizzas Sold: The sum of the quantities of all pizzas sold.
4. Tota Orders: The total number of orders placed.
5. Average Pizzas Per Order: The average number of pizzas sold per order, calculated by dividing the total number of pizzas sold by the total number of orders.

- CHARTS REQUIREMENT

We would like to visualize various aspects of our pizza sales data to gain insights and understand key trends. We have identified the following requirements for creating charts:
1. Daily Trend for Total Orders:
Create a bar chart That displays the daily trend orders over a specific time period. This chart will help us identify any patterns or fluctuations in order volumes on a daily basis.

2. Monthly Trend for Total Orders:
Create a line chart that illustrates the monthly trend of total orders throughout the day. This chart will aloow us to identify which month has highest order.

3. Percentage of Sales by Pizza Category:
Create a pie chart that shows the distribution of sales across different pizza categories. This chart will provide insights into the popularity of various pizza categories and their contribution to overall sales.

4. Percentage od Sales by Pizza Size:
Generate a pie cart that represents the percentage of sales attributed to different pizza sizes. This chart will helo us understand customer preferencens for pizza sizes and their impact on sales.

5. Total Pizzas Sold by Pizza Category:
Create a funnel chart that presents the total number of pizzas sold for each pizza category. This chart will allow us to compare the sales performance of different pizza categories.

6. Top 5 Best Sellers by Revenue, Total Quantity and Total Orders:
Create a bar chart highlighting the top 5 best-selling pizzas based on the Revenue, Total Quantity and Total Orders. This chart will help us identify the most popular pizza options.

7. Bottom 5 Worst Sellers by Revenue, Total Quantity and Total Orders:
Create a bar chart highlighting the bottom 5 worst-selling pizzas based on the Revenue, Total Quantity and Total Orders. This chart will help us identify the less popular pizza options.

- Details of Pizza Sales data

The database has one table which contains 12 columns - pizza_id, order_id, pizza_name_id, quantity, order_date, order_time, unit_price, total_price, pizza_size, pizza_category, pizza_ingredients, pizza_name. 

- PIZZA SALES SQL QUERIES

A. KPI’s
1. Total Revenue:
SELECT SUM(total_price) AS Total_Revenue FROM pizza_sales

![Picture1](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/914079fd-abdd-41f0-8494-0632ead6c57f)

2. Average Order Value
SELECT (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_order_Value FROM pizza_sales

![Picture2](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/50a8a8f2-6f58-4a64-a49e-bea728dc53bc)

3. Total Pizzas Sold
SELECT SUM(quantity) AS Total_pizza_sold FROM pizza_sales

![Picture3](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/c101160d-894b-404c-bb98-b9a92fce0fe3)

4. Total Orders
SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales

![Picture4](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/c0341ee5-72e7-4a07-a63a-046039b00a13)

5. Average Pizzas Per Order
SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) / 
CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2))
AS Avg_Pizzas_per_order
FROM pizza_sales

![Picture5](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/f35b3814-e27b-4d0e-a487-3b888e976e96)

B. Daily Trend for Total Orders

SELECT DATENAME(DW, order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders 
FROM pizza_sales
GROUP BY DATENAME(DW, order_date)
Output:

![Picture6](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/9b880310-314e-4bf1-9157-40f6a2afb556)

C. Monthly Trend for Orders

select DATENAME(MONTH, order_date) as Month_Name, COUNT(DISTINCT order_id) as Total_Orders
from pizza_sales
GROUP BY DATENAME(MONTH, order_date)
Output:

![Picture7](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/dc1104a1-3c16-408f-bb1c-c75624efd327)

C. Hourly Trend for Orders-What is our peak hour?

select datepart(hh,order_time) as hour,count(distinct order_id) as Total_Orders
from [dbo].[pizza_sales] group by datepart(hh,order_time) order by Total_Orders desc
Output:

![Picture8](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/5b498cb7-f0e0-4d6f-a9af-837fdcd2c4e8)

D. % of Sales by Pizza Category

SELECT pizza_category, CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_category
Output:

![Picture9](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/f069e1ee-4f04-41cd-aef4-5aef6f10dd65)

E. % of Sales by Pizza Size

SELECT pizza_size, CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_size
ORDER BY pizza_size
Output:

![Picture10](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/6b37e03a-b2d3-4800-8bf6-14c7bdc82432)

F. Total Pizzas Sold by Pizza Category

SELECT pizza_category, SUM(quantity) as Total_Quantity_Sold
FROM pizza_sales
WHERE MONTH(order_date) = 2
GROUP BY pizza_category
ORDER BY Total_Quantity_Sold DESC
Output:

![Picture11](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/6b71ece3-69ae-4365-ae53-9c7599eb5cbd)

G. Top 5 Pizzas by Revenue

SELECT Top 5 pizza_name, SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue DESC
Output:

![Picture12](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/91d855ec-e67d-47a7-8135-82d3b1096e81)

H. Bottom 5 Pizzas by Revenue

SELECT Top 5 pizza_name, SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue ASC
Output:

![Picture13](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/cacec6ac-8066-425f-a8ba-d6fd68c89d05)

I. Top 5 Pizzas by Quantity

SELECT Top 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold DESC
Output:

![Picture14](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/3ab1d34b-078d-4560-929a-e55e37aae7fd)

J. Bottom 5 Pizzas by Quantity
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold ASC
Output:

![Picture15](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/e4093a1b-dda1-49f4-b370-bf890e4b55d1)


# Pizza Sales Insights in PowerBI
- PIZZA SALES DAX QUERIES IN POWERBI 

New measures were created to

1. find total sales/revenue.
Total Revenue = SUM(pizza_sales[total_price]) 

2. get total pizza sold 
Total Pizzas Sold = sum(pizza_sales[quantity])

3. find the total number of orders 
Total Order = DISTINCTCOUNT(pizza_sales[order_id])

4. get average order value
Average Order Value = [Total Revenue]/[Total Order] 

5. find the average pizza sold per order
Average Pizzas Sold Per Order = [Total Pizzas Sold]/[Total Order]

Another 2 measures were created to get the first 3 letters of month and day name to represent in the monthly and daily trending chart.

-Order Day = UPPER(LEFT(pizza_sales[Day Name],3))

-Order Month = UPPER(LEFT(pizza_sales[Month Name],3))

Pizza Category and Order date were added as filters/slicers.

2 buttons were added to navigate from one to another.


# Report Snapshot (Power BI DESKTOP)
Home Page
![Screenshot (3)](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/84cf7f11-60e5-4b42-a147-18ce5d44dc93)

Best/Worst Seller Page
![Screenshot (4)](https://github.com/Lab1ba/PizzaSalesInsights/assets/100675446/37d49668-34f2-41ee-932e-292a21a31c03)

 From the dashboard, the KPI’s values are exactly the same as the values got from SQL queries. 

 --Decisions From the Dashboard are
 - Orders are highest on weekends, Friday/Saturday evenings.
 - There are maximum orders from month of July and January.
 - Classis Category contributes to maximum sales & total orders.
 - Large size pizza contributes to maximum sales.
 - The Thai Chicken Pizza contributes to maximum revenue.
 - The Classic Deluxe Pizza contributes to maximum quantities.
 - The Classic Deluxe Pizza contributes to maximum total orders.
 - The Brie Carre Pizza contributes to minimum revenue.
 - The Brie Carre Pizza contributes to minimum quantities.
 - The Brie Carre Pizza contributes to minimum total orders.
