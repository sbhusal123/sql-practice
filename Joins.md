# Joins In Sql:

- [Playground](https://www.programiz.com/sql/online-compiler/)

We've a table as follows:

**Customers**
- customer_id [int]
- first_name [varchar(100)]
- last_name [varchar(100)]
- age [int]
- country [varchar(100)]

**Orders**
- order_id [integer]
- item [varchar(100)]
- amount [integer]
- customer_id [integer] (FK: customer_id)

**Shippings**
- shipping_id [integer]
- status [integer]
- customer [integer] (FK: customer_id)


## 1. Write a sql query to find the details of a customer who has no order.

**a. With left join**
```sql
select customer_id, first_name, last_name, age, country from
(select * from Customers c left join Orders o on c.customer_id = o.customer_id
except
select * from Customers c inner join Orders o on c.customer_id = o.customer_id);
```
**b. With except and sub query**
```sql
select * from Customers where customer_id = (
select customer_id from Customers
except
select customer_id from Orders);
```

## 2. Select the first_name, last_name, and order count of the customer in descending order who has ordered more than 2 times.

**With Direct Joins Of Table**
```sql
select c.first_name as first_name, c.last_name,  count(*) as order_count
from Customers c, Orders o
on c.customer_id = o.customer_id
group by c.customer_id having count(*) > 2 order by count(*) desc;
```

**With Subquery Join**
```sql
select c.first_name, c.last_name, temp_table.order_count from 
Customers c inner join
(select customer_id, count(*) as order_count 
from Orders 
group by customer_id 
having count(*) > 2 
order by count(*) desc) as temp_table
on c.customer_id = temp_table.customer_id;
```

## 3. Find the customers who have no orders but a shipping

```sql
select c.* from Customers c left join
(
  select c.customer_id as customer_id
  from
  Customers c inner join Orders o
  on c.customer_id = o.customer_id
) customerOrder
on
c.customer_id = customerOrder.customer_id
except
select c.* from Customers c inner join
(
  select c.customer_id as customer_id
  from
  Customers c inner join Shippings s
  on c.customer_id = s.customer
) customerShipping
on
c.customer_id = customerShipping.customer_id;
```

