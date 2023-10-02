## northwind.db

The northwind.db has the following Entity Relationship Diagram which we have to keep in mind before solving the questions.

![erd](https://github.com/ProPrakhar332/sql-practice/blob/main/nothwind/northwind-schema.jpg)

---

### Section1: Easy

---

<h3>1. Show the category_name and description from the categories table sorted by category_name.</h3>

```sql
SELECT   category_name, description

FROM     categories

ORDER BY category_name;
```

<h3>2. Show all the contact_name, address, city of all customers which are not from 'Germany', 'Mexico', 'Spain'.</h3>

```sql
SELECT   contact_name, address, city

FROM     customers

WHERE    country NOT IN ('Germany', 'Mexico', 'Spain');
```

<h3>3. Show order_date, shipped_date, customer_id, Freight of all orders placed on 2018 Feb 26.</h3>

```sql
SELECT   order_date, shipped_date,customer_id, freight

FROM     orders

WHERE    order_date = '2018-02-26';
```

<h3>4. Show the employee_id, order_id, customer_id, required_date, shipped_date from all orders shipped later than the required date.</h3>

```sql
SELECT   employee_id, order_id, customer_id, required_date, shipped_date

FROM     orders

WHERE    shipped_date > required_date;
```

<h3>5. Show all the even numbered Order_id from the orders table.</h3>

```sql
SELECT   order_id

FROM     orders

WHERE    order_id % 2 = 0;
```

<h3>6. Show the city, company_name, contact_name of all customers from cities which contains the letter 'L' in the city name, sorted by contact_name
</h3>

```sql
SELECT   city,company_name,contact_name 

FROM     customers

WHERE    city LIKE "%l%"

ORDER BY contact_name;
```

<h3>7. Show the company_name, contact_name, fax number of all customers that has a fax number. (not null)
</h3>

```sql
SELECT  company_name, contact_name, fax

FROM    customers

WHERE   fax IS NOT NULL;
```

<h3>8. Show the first_name, last_name. hire_date of the most recently hired employee.
</h3>

Solution 1:-

```sql
SELECT  first_name, last_name, hire_date

FROM    employees

ORDER BY hire_date DESC

LIMIT 1;
```

Solution 2:-

```sql
SELECT  first_name, last_name, MAX(hire_date) AS hire_date

FROM    employees;
```

<h3>9. Show the average unit price rounded to 2 decimal places, the total units in stock, total discontinued products from the products table.
</h3>

```sql
SELECT  ROUND(AVG(unit_price),2) , SUM(units_in_stock), SUM(discontinued)

FROM    products
```

---

### Section2: Medium

---

<h3>1. Show the ProductName, CompanyName, CategoryName from the products, suppliers, and categories table
</h3>

```sql
SELECT  P.product_name, S.company_name, C.category_name

FROM    products P LEFT JOIN suppliers S,categories C

ON      P.supplier_id = S.supplier_id AND

        P.category_id = C.category_id
```

<h3>2. Show the category_name and the average product unit price for each category rounded to 2 decimal places.
</h3>

```sql
SELECT  C.category_name, ROUND(AVG(P.unit_price),2)

FROM    products P LEFT JOIN categories C 

ON      P.category_id = C.category_id

GROUP BY C.category_name
```

<h3>3. Show the city, company_name, contact_name from the customers and suppliers table merged together.
Create a column which contains 'customers' or 'suppliers' depending on the table it came from.</h3>

```sql
SELECT   city, company_name, contact_name, "customers" AS relation FROM customers

UNION

SELECT   city, company_name, contact_name, "suppliers" AS relation FROM suppliers
```

---

### Section3: Hard

---

<h3>1. Show the employee's first_name and last_name, a "num_orders" column with a count of the orders taken, and a column called "Shipped" that displays "On Time" if the order shipped on time and "Late" if the order shipped late.

Order by employee last_name, then by first_name, and then descending by number of orders.</h3>

```sql
SELECT   E.first_name, E.last_name, COUNT(*) AS num_orders,
         CASE WHEN O.required_date > O.shipped_date THEN "On Time" ELSE "Late" END AS shipped

FROM     orders O LEFT JOIN employees E

ON       O.employee_id = E.employee_id

GROUP BY E.employee_id,shipped
ORDER BY E.last_name, E.first_name, num_orders DESC
```
