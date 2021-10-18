# Join

## Self Join

<https://www.sqlservertutorial.net/sql-server-basics/sql-server-self-join/>

- Using self join to query hierarchical data. Replace the `INNER JOIN` clause by `LEFT JOIN` clause to get the result set that includes `Fabiola Jackson` in the employee column.

```sql
SELECT
    e.first_name + ' ' + e.last_name employee,
    m.first_name + ' ' + m.last_name manager
FROM
    sales.staffs e
INNER JOIN sales.staffs m ON m.staff_id = e.manager_id
ORDER BY
    manager;
```

- Using self join to compare rows within a table.

```sql
SELECT
    c1.city,
    c1.first_name + ' ' + c1.last_name customer_1,
    c2.first_name + ' ' + c2.last_name customer_2
FROM
    sales.customers c1
INNER JOIN sales.customers c2 ON c1.customer_id > c2.customer_id
AND c1.city = c2.city
ORDER BY
    city,
    customer_1,
    customer_2;
```
