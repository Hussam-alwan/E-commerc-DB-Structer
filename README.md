# E-commerc DB Structer
The design and the analysis for the DB based on the book "Practical Web Database Design"
### Session 3 (2.1) Task:
#### ERD
![image](https://github.com/user-attachments/assets/bfd196b3-b7ec-409e-9bed-1249ba5b01dc)
 [Table Creation Script](https://github.com/Hussam-alwan/E-commerc-DB-Structer/blob/main/Tables)

##  Queries
Write an SQL query to generate a daily report of the total revenue for a specific date.
```
Select Orders.order_date as report_date ,sum(Orders.total_amout)
from Orders
where Orders.order_date='2024-12-12'
```
Write an SQL query to generate a monthly report of the top-selling products in a given month.
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

Write a SQL query to retrieve a list of customers who have placed orders totaling more than $500 in the past month.
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

