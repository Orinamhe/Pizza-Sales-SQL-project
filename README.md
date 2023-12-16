# Pizza-Sales-SQL-project
contains SQL query, questions, insights, reports and Power BI dashboards 

![image](https://github.com/Orinamhe/Pizza-Sales-SQL-project/assets/153821560/6005d513-2e83-4ba9-b373-8300ed236b44)


QUESTIONS

KPIs
1.	What is the total revenue?
2.	What is the total orders
3.	What is the total sales
4.	What is the average number of pizzas ordered


Get the trends (hourly, daily, monthly) of important KPIs
Information about categories and sizes of pizzas 
What pizzas are the most and least ordered in the restaurant.

INSIGHTS
The pizza size and categories provide some information on the preference of customers, with classic pizzas the most ordered. Large pizzas are the most ordered an sold with Extra large contributing a very little amount of orders/sales, and thus having this in the menu proves to provide very little value for money.

Peak time of sales is between noon and 1:00pm (lunch) and then 4:00 – 7:00pm (close of work), and thus having more employees around these peak times to process orders and make pizzas can make the restaurant more efficient.

Based on daily trends, most sales come between Thursday and Saturday, with sales peaking on Friday. The monthly trend observed in the data could help in making plans for the following year, matching the workforce with months with increased demand for pizzas.

SQL QUERIES
Total revenue made
SELECT SUM (total_price) AS total_revenue FROM pizza;

Total pizza orders in the given period
SELECT COUNT (DISTINCT order_id) AS total_orders FROM pizza;

Total pizza sales
SELECT SUM (quantity) AS total_pizza_sold FROM pizza;

Average number of pizzas per order
SELECT SUM (quantity) :: FLOAT/COUNT (DISTINCT order_id) AS Average_pizza_per_order FROM pizza;
SELECT CAST (SUM (quantity) AS DECIMAL)/COUNT (DISTINCT order_id) AS Average_pizza_per_order
FROM pizza;

Hourly sales and orders from opening to closing
SELECT
TO_CHAR (order_time, 'HH24') AS hourly_period,
COUNT (DISTINCT order_id) AS total_orders,
SUM (quantity) AS total_sales
FROM pizza
GROUP BY TO_CHAR (order_time, 'HH24') ORDER BY hourly_period;

Daily sales and orders
SELECT TO_CHAR (order_date, 'Day'),
COUNT (DISTINCT order_id) AS Total_orders,
SUM (quantity) AS total_sales GROUP BY TO_CHAR (order_date, 'Day')

Monthly sales and orders trend
SELECT TO_CHAR (order_date, 'Mon') AS Month_name, COUNT (DISTINCT order_id) AS total_orders,
SUM (quantity) AS total_sales FROM pizza
GROUP BY TO_CHAR (order_date, 'Mon') ORDER BY total_orders DESC
FROM pizza

Sales and revenue based on pizza category
SELECT pizza_category, SUM(quantity) AS total_sales, SUM (total_price) AS total_revenue FROM pizza
GROUP BY pizza_category;

% sales and revenue based on category
SELECT pizza_category,
SUM (total_price) AS Total_revenue,
SUM (total_price) * 100/ (SELECT SUM(total_price) FROM pizza) AS
percentage_total_sales FROM pizza
GROUP BY pizza_category;

% sales and revenue based on pizza size
SELECT pizza_size,
ROUND(SUM (total_price)) AS Total_revenue,
SUM (total_price) * 100/ (SELECT SUM(total_price) FROM pizza) AS percentage_total_sales FROM pizza
GROUP BY pizza_size
ORDER BY percentage_total_sales;

Most ordered and least ordered pizza during the period, along with sales and revenue
SELECT pizza_name,
SUM (total_price) AS total_revenue,
COUNT (DISTINCT order_id) AS total_orders, SUM (quantity) AS total_sales
FROM pizza
GROUP BY pizza_name ORDER BY total_revenue DESC LIMIT 5;

SELECT pizza_name,
SUM (total_price) AS total_revenue,
COUNT (DISTINCT order_id) AS total_orders, SUM (quantity) AS total_sales
FROM pizza
GROUP BY pizza_name ORDER BY total_revenue ASC LIMIT 5;
