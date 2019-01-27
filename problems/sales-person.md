

## Solution
---
#### Approach: Using `OUTER JOIN` and `NOT IN` [Accepted]

**Intuition**

If we know all the persons who have sales in this company 'RED', it will be fairly easy to know who do not have.

**Algorithm**

To start, we can query the information of sales in company 'RED' as a temporary table. And then try to build a connection between this table and the **salesperson** table since it has the name information.

```sql
SELECT
    *
FROM
    orders o
        LEFT JOIN
    company c ON o.com_id = c.com_id
WHERE
    c.name = 'RED'
;
```
>Note: "LEFT OUTER JOIN" could be written as "LEFT JOIN".

```
| order_id | date     | com_id | sales_id | amount | com_id | name | city   |
|----------|----------|--------|----------|--------|--------|------|--------|
| 3        | 3/1/2014 | 1      | 1        | 50000  | 1      | RED  | Boston |
| 4        | 4/1/2014 | 1      | 4        | 25000  | 1      | RED  | Boston |
```

Obviously, the column *sales_id* exists in table **salesperson** so we may use it as a subquery, and then utilize the [`NOT IN`](https://dev.mysql.com/doc/refman/5.7/en/any-in-some-subqueries.html) to get the target data.


**MySQL**

```sql
SELECT
    s.name
FROM
    salesperson s
WHERE
    s.sales_id NOT IN (SELECT
            o.sales_id
        FROM
            orders o
                LEFT JOIN
            company c ON o.com_id = c.com_id
        WHERE
            c.name = 'RED')
;
```
