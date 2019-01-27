

## Solution
---
#### Approach: Using `case...when...` [Accepted]

**Algorithm**

In Math, three segments can form a triangle only if the sum of any of the two segments is larger than the third one.
(In other words, the subtraction of any of the two segments are smaller than the third one.)

So, we can use this knowledge to judge with the help of the MySQL control statements [`case...when...`](https://dev.mysql.com/doc/refman/5.7/en/case.html).

**MySQL**

```sql
SELECT 
    x,
    y,
    z,
    CASE
        WHEN x + y > z AND x + z > y AND y + z > x THEN 'Yes'
        ELSE 'No'
    END AS 'triangle'
FROM
    triangle
;
```
