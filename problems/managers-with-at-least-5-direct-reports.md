

## Solution
---
#### Approach: Using `JOIN` and a temporary table [Accepted]

**Algorithm**

First, we can get the Id of the manager having more than 5 direct reports just using this *ManagerId* column.

Then, we can get the name of this manager by join that table with the **Employee** table.

**MySQL**

```sql
SELECT
    Name
FROM
    Employee AS t1 JOIN
    (SELECT
        ManagerId
    FROM
        Employee
    GROUP BY ManagerId
    HAVING COUNT(ManagerId) >= 5) AS t2
    ON t1.Id = t2.ManagerId
;
```
