**Product**(_pid_, pname, price, description)

**Customer**(_cid_,cname, address)

**Sells**(_cid_, quantity)

> 1. Retrive the record of product which were sold to customer id 12.

**Using Nested Query**

```sql
select *
from
Product
where pid in
    (select pid from sells where cid = 12)
```

**Using Joins**

```sql
select p.* from
Product p inner join sells s
on
p.pid = s.pid and s.cid = 12
```

> 2. Create above tables:

_Syntax:_

```sql
CREATE TABLE table_name (
    column_1 data_type column_constraint,
    column_2 data_type column_constraint,
    ...
    --- optional, this is comment in sql
    constraint <constraint_name> check (...)
 );
```

```sql
create table Product (
    pid int primary key,
    pname varchar(50) not null unique,
    description varchar(100)
)

create table Customer (
    cid int primary key,
    name varchar(20),
    address varchar(50)
)

--- creating relation with foreign key
create table Sells (
    pid int foreign key references Product(pid),
    cid int foreign key references Customer(cid),
    quantity int,
    constraint chk_qty check(quantity > 0)
)
```

> **Note: No need to specify int size in oracle.**

> 3. Find the product whose sells quantity is max.

```sql
select p.* from Product p  natural join -- join Product table with
(select max(sell_qty) as max_sell, pid from -- select pid corresponding to max sell_qty
    (select pid,sum(quantity) as sell_qty from sells group by pid) -- calculate the qty sold for each product
)
```

> 4. Find total no of customers whose name starts with S.

```sql
select count(*) from Customer where cname like 'S%' --- for ending with s: like 'S%'
```
