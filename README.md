# Pizza-Sales-Analysis

## Background

For the Maven Pizza Challenge, you’ll be playing the role of a BI Consultant hired by Plato's Pizza, a Greek-inspired pizza place in New Jersey. You've been hired to help the restaurant use data to improve operations, and just received the following note:

Welcome aboard, we're glad you're here to help!

Things are going OK here at Plato's, but there's room for improvement. We've been collecting transactional data for the past year, but really haven't been able to put it to good use. Hoping you can analyze the data and put together a report to help us find opportunities to drive more sales and work more efficiently.

Here are some questions that we'd like to be able to answer:

•	What days and times do we tend to be busiest?

•	How many pizzas are we making during peak periods?

•	What are our best and worst selling pizzas?

•	What's our average order value?

•	How well are we utilizing our seating capacity? (we have 15 tables and 60 seats)

That's all I can think of for now, but if you have any other ideas I'd love to hear them – you're the expert!

Thanks in advance,
Mario Maven (Manager, Plato's Pizza)

### About the Data

The dataset contains 4 tables in CSV format

•	The Orders table contains the date & time that all table orders were placed

•	The Order Details table contains the different pizzas served with each order in the Orders table, and their quantities

•	The Pizzas table contains the size and price for each distinct pizza in the Order Details table, as well as its broader pizza type

•	The Pizza Types table contains details on the pizza types in the Pizzas table, including their name as it appears on the menu, the category it falls under, and its list of ingredients

### Additional Questions to Answer

Here are some questions that we'd like to be able to answer:

•	What are the total sales, total number of orders and total number of customers?

•	What is the sales revenue per month?

•	What is the sales revenue per day?

•	What is the average customer per day? 

•	What is the average number of pizzas per order? 

•	Which pizzas generate the highest and lowest revenue?

•	Monthly Sales and Order Trend

## SQL Queries
Create View:
CREATE VIEW Pizza_Sales AS
SELECT order_details.order_details_id, 
	order_details.order_id,
	order_details.pizza_id,
	order_details.quantity,
	orders.date,
	orders.time,
	pizza_types.category,
	pizza_types.ingredients,
	pizza_types.name,
	pizzas.size,
	pizzas$.price
FROM order_details
LEFT JOIN orders
	ON order_details.order_id = orders.order_id
LEFT JOIN pizzas
	ON order_details.pizza_id = pizzas.pizza_id
LEFT JOIN pizza_types
	ON pizzas.pizza_type_id = pizza_types.pizza_type_id

Revenue:
SELECT ROUND (SUM((quantity * price)), 0) AS Revenue
FROM Pizza_Sales
 
Order:
SELECT ROUND (SUM(quantity), 0) AS TotalOrders
FROM Pizza_Sales

Customers:
SELECT COUNT(DISTINCT(order_id)) AS TotalCustomers
FROM Pizza_Sales

Ave. Revenue per Month:
SELECT ROUND (SUM(quantity*price)/COUNT(DISTINCT(Month(date))), 0) AS Ave_Monthly_Revenue
FROM Pizza_Sales

Ave. Revenue per Day:
SELECT ROUND (SUM(quantity*price)/COUNT(DISTINCT(date)), 0) AS Ave_Daily_Revenue
FROM Pizza_Sales

Ave. Number of Customers per Day:
SELECT ROUND(COUNT(DISTINCT(order_id))/COUNT(DISTINCT(date)), 0) AS Ave_CustperDay
FROM Pizza_Sales

Ave. Order Value:
SELECT ROUND (SUM(quantity*price)/COUNT(DISTINCT(order_id)), 2) AS Ave_OderValue
FROM Pizza_Sales
 

Ave. Order:
SELECT ROUND(SUM(quantity)/COUNT(DISTINCT(order_id)), 2) AS Ave_Order
FROM Pizza_Sales

Top Pizza Sales:
SELECT TOP(5) ROUND (SUM((quantity * price)), 0) AS Revenue, name	
FROM Pizza_Sales
GROUP BY (name)
ORDER BY (Revenue) DESC

Bottom Pizza Sales:
SELECT TOP(5) ROUND (SUM((quantity * price)), 0) AS Revenue, name	
FROM Pizza_Sales
GROUP BY (name)
ORDER BY (Revenue)

Top Ordered Pizza:
SELECT TOP(5) ROUND (SUM(quantity), 0) AS TotalOrders, name
FROM Pizza_Sales
GROUP BY (name)
ORDER BY (TotalOrders) DESC

Bottom Ordered Pizza:
SELECT TOP(5) ROUND (SUM(quantity), 0) AS TotalOrders, name
FROM Pizza_Sales
GROUP BY (name)
ORDER BY (TotalOrders) DESC

Monthly Order and Revenue:
SELECT MONTH (date) AS Month_num,
		DATENAME (MONTH, date) AS Month_Name,
		ROUND (SUM(quantity), 0) AS TotalOrders, 
		ROUND (SUM((quantity * price)), 0) AS Revenue
FROM [Pizza_Sales]
GROUP BY MONTH (date), DATENAME (MONTH, date)
ORDER BY MONTH (date)

Day with Highest Revenue:
SELECT DATENAME(WEEKDAY, date) AS Week_Day,
	ROUND (SUM(quantity), 0) AS TotalOrders, 
	ROUND (SUM((quantity * price)), 0) AS Revenue
FROM [Pizza_Sales]
GROUP BY DATENAME(dw, date)
ORDER BY Revenue DESC

Hourly Order and Revenue:
SELECT DATEPART(HOUR, time) AS Hour_Int,
ROUND (SUM(quantity), 0) AS TotalOrders, 
ROUND (SUM((quantity * price)), 0) AS Revenue
FROM [Pizza_Sales]
GROUP BY DATEPART(HOUR, time)
ORDER BY DATEPART(HOUR, time)


## Answers to the Questions
1.	What days and times do we tend to be busiest?
Ans: 
Busiest Day is Friday followed by Saturday and Thursday
Busiest Times are between 12nn to 1pm and 5pm to 6pm

2.	How many pizzas are we making during peak periods?
Ans: 
12nn – 6776
1pm – 6413
5pm – 5211
6pm – 5417

3.	What are our best and worst selling pizzas?
Ans:
Best Selling are – The Classic Deluxe Pizza, The Barbecue Chicken Pizza, The Hawaiian Pizza; 
Worst Selling is – The Brie Carre Pizza

4.	What's our average order value?
Ans: $38.31

5.	How well are we utilizing our seating capacity? (we have 15 tables and 60 seats)
Ans:  
Based on the hourly customer volume data and the assumption of one customer per table, seat utilization with 15 tables appears generally adequate. However, closer monitoring is recommended during peak hours between 12 PM to 1 PM and 4 PM to 7 PM, where customer traffic is high and potentially exceeds seating capacity.
The data also shows 5 days on peak hours during the busiest days (Thursdays, Fridays, and Saturdays) where seat utilization might surpass 100%. This suggests occasional overcrowding during these peak periods.

6.	What are the total sales, total number of orders and total number of customers?
Ans:
Total Sales - $817,860; 
Total Orders – 49, 574; 
Total Customers – 21, 350;

7.	What is the sales revenue per month?
Ans: $68,155

8.	What is the sales revenue per day?
Ans: $2,285

9.	What is the average customer per day?
Ans: 59

10.	What is the average number of pizzas per order?
Ans: 2

11.	 Which pizzas generate the highest and lowest revenue?
Ans.
Highest – The Thai Chicken Pizza with $43,434 sales; 
Lowest – The Brie Carre Pizza with $11,588 sales

12.	Monthly Sales and Order Trend
The month with the highest revenue sales ($72,558) and order (4392) is July.
The month with the lowest revenue sales ($64,028) and order (3883) is October.

## Tableau Visualization
Pizza Sales Dashboard

![image](https://github.com/mel4data/Pizza-Sales-Analysis/assets/170362474/9b249104-cb8f-47f4-a34f-acbb745355ce)

## Recommendations
•	Consider implementing a waitlist system and ensure adequate staff during peak hours to manage customer expectations and improve customer satisfaction.

•	Menu Optimization – consider removing the XXL size for the Greek Pizza due to low demand and adding an XL size for the following due to high demand: The Classic Deluxe Pizza, The Barbecue Chicken Pizza, The Hawaiian Pizza, The Pepperoni Pizza, The Thai Chicken Pizza, The California Chicken Pizza, and The Five Cheese Pizza

•	Customer feedback data would help in menu optimization and enhancing customer satisfaction and experience.

•	Promotions and Marketing:
  Bundle Deals – combo meals can be implemented to encourage large orders and increase revenue.
  Customer Demographics data help in improving marketing and promotions to attract new customers.

