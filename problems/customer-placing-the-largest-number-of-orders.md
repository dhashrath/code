

## Solution
---
#### Approach: Using `LIMIT` [Accepted]

**Algorithm**

First, we can select the <b>customer_number</b> and the according count of orders using `GROUP BY`.
```sql
SELECT
    customer_number, COUNT(*)
FROM
    orders
GROUP BY customer_number
```

| customer_number | COUNT(*) |
|-----------------|----------|
| 1               | 1        |
| 2               | 1        |
| 3               | 2        |

Then, the <b>customer_number</b> of first record is the result after sorting them by order count descending.

| customer_number | COUNT(*) |
|-----------------|----------|
| 3               | 2        |

In MySQL, the [LIMIT](https://dev.mysql.com/doc/refman/5.7/en/select.html) clause can be used to constrain the number of rows returned by the SELECT statement. It takes one or two nonnegative numeric arguments, the first of which specifies the offset of the first row to return, and the second specifies the maximum number of rows to return. The offset of the initial row is 0 (not 1).

It can be used with only one argument, which specifies the number of rows to return from the beginning of the result set. So `LIMIT 1` will return the first record.

**MySQL**

```sql
SELECT
    customer_number
FROM
    orders
GROUP BY customer_number
ORDER BY COUNT(*) DESC
LIMIT 1
;
```
