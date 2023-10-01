## northwind.db

The northwind.db has the following Entity Relationship Diagram which we have to keep in mind before solving the questions.

![erd](https://github.com/ProPrakhar332/sql-practice/blob/main/hospital/hospital-schema.jpg)

---

### Section1: Easy

---

<h3>1. Show the category_name and description from the categories table sorted by category_name.</h3>

```sql
SELECT   category_name, description

FROM     categories

ORDER BY category_name
```

<h3>2. Show all the contact_name, address, city of all customers which are not from 'Germany', 'Mexico', 'Spain'.</h3>

```sql
SELECT   contact_name, address, city

FROM     customers

WHERE    country NOT IN ('Germany', 'Mexico', 'Spain')
```

<h3>3. Show order_date, shipped_date, customer_id, Freight of all orders placed on 2018 Feb 26.</h3>

```sql
SELECT   order_date, shipped_date,customer_id, freight

FROM     orders

WHERE    order_date = '2018-02-26'
```

<h3>4. Show the employee_id, order_id, customer_id, required_date, shipped_date from all orders shipped later than the required date.</h3>

```sql
SELECT   employee_id, order_id, customer_id, required_date, shipped_date

FROM     orders

WHERE    shipped_date > required_date
```

<h3>5. Show all the even numbered Order_id from the orders table.</h3>

```sql
SELECT   order_id

FROM     orders

WHERE    order_id%2=0
```
