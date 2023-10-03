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
