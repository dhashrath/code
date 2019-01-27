

## Solution
---
#### Approach: Using `JOIN` and `DATEDIFF()` clause [Accepted]

**Algorithm**

MySQL uses [DATEDIFF](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_datediff) to compare two date type values.

So, we can get the result by joining this table **weather** with itself and use this `DATEDIFF()` function.

**MySQL**

```sql
SELECT
    weather.id AS 'Id'
FROM
    weather
        JOIN
    weather w ON DATEDIFF(weather.date, w.date) = 1
        AND weather.Temperature > w.Temperature
;
```
