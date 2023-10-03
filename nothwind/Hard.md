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
