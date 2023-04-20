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


