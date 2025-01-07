# E-commerce Database Design and Analysis
Inspired by the principles outlined in the book "Practical Web Database Design," this e-commerce database structure aims to facilitate efficient data management and robust 
analysis. The design includes well-defined tables and relationships to cover essential aspects of an e-commerce platform:
### Session 3 Task:
#### ERD
![image](https://github.com/user-attachments/assets/bfd196b3-b7ec-409e-9bed-1249ba5b01dc)
 [Table Creation Script](https://github.com/Hussam-alwan/E-commerc-DB-Structer/blob/main/Tables)

##  Queries
#### Write an SQL query to generate a daily report of the total revenue for a specific date.
```
Select Orders.order_date as report_date ,sum(Orders.total_amout)
from Orders
where Orders.order_date='2024-12-12'
```
#### Write an SQL query to generate a monthly report of the top-selling products in a given month.
```
Select Product.name , sum(od.quantity) as total_quantity_sold, total_quantity_sold *unit_pice as total revenue
from Order_detail od
join
Orders on od.order_id=Orders.id
join 
Product on od.product_id= Product.id
where DATE_FORMAT(o.order_date, '%Y-%m') = '2024-12'
Group by Product.name
order by total_quantity_sold DESC
```
#### Write a SQL query to retrieve a list of customers who have placed orders totaling more than $500 in the past month.
Include customer names and their total order amounts.

```
Select Customer.first_name,Customer.last_name,sum(total_amount) as total_order_amount
from Customer 
join Orders on Orders.customer_id =Customer.id
where Orders.order_date >= DATE_SUB(CURDATE(), INTERVAL 1 MONTH
group by Customer.id,Customer.first_name,Customer.last_name
having total_order_amount >500
ORDER BY total_order_amount DESC
```

### How we can apply a denormalization mechanism on customer and order entities ?
We can create a new table called sales_orders  that includes customer information along with the order details.
```
CREATE TABLE sales_orders ( order_id INT PRIMARY KEY, order_date DATE, total_amount
FLOAT, customer_id INT, customer_first_name VARCHAR(50), customer_last_name VARCHAR(50),
customer_email VARCHAR(100) );

```
We'll populate this new table with data from the existing Orders and Customer tables

```
INSERT INTO sales_orders (order_id, order_date, total_amount, customer_id,
customer_first_name, customer_last_name, customer_email) SELECT o.id, o.order_date,
o.total_amount, c.id, c.first_name, c.last_name, c.email FROM Orders o JOIN Customers c ON
o.customer_id =c.id

```

### Session 4 Task:

##  Queries
 #### Write a SQL query to search for all products with the word "camera" in either the product name or description

```
Select Product.name
from Product
where Product.name like '%camera%' or Product.discption like '%camera%'
```
#### Can you design a query to suggest popular products in the same category for the same author, 
excluding the Purchsed product from the recommendations?


```
select p.name, p.description,p.p rice
from Product p
join Order_datail od on od.product_id=p.id
join Orders on od.order_id = Orders.id
join Customer on Orders.customer_id= Customer.id
join Product p2 on p.category_id=p2.category_id
where Customer.id = 1
and p.category_id=(select category_id from Product where id= 2)
and p.id != 2
group by p.id
order by SUM(od.quantity) DESC
```

