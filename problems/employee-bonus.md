

## Solution
---
#### Approach: Using `OUTER JOIN` and `WHERE` clause [Accepted]

**Intuition**

Join table **Employee** with **Bonus** and then use `WHERE` clause to get the required records.

**Algorithm**

Since foreign key **Bonus.empId** refers to **Employee.empId** and some employees do not have bonus records, we can use `OUTER JOIN` to link these two tables as the first step.

```sql
SELECT
    Employee.name, Bonus.bonus
FROM
    Employee
        LEFT OUTER JOIN
    Bonus ON Employee.empid = Bonus.empid
;
```
>Note: "LEFT OUTER JOIN" could be written as "LEFT JOIN".

The output to run this code with the sample data is as below.

```
| name   | bonus |
|--------|-------|
| Dan    | 500   |
| Thomas | 2000  |
| Brad   |       |
| John   |       |
```
The bonus value for 'Brad' and 'John' is empty, which is actually `NULL` in the database. "Conceptually, NULL means “a missing unknown value” and it is treated somewhat differently from other values." Check the [Working with NULL Values](https://dev.mysql.com/doc/refman/5.7/en/working-with-null.html) in MySQL manual for more details. In addition, we have to use `IS NULL` or `IS NOT NULL` to compare a value with `NULL`.

At last, we can add a `WHERE` clause with the proper conditions to filter these records.

**MySQL**

```sql
SELECT
    Employee.name, Bonus.bonus
FROM
    Employee
        LEFT JOIN
    Bonus ON Employee.empid = Bonus.empid
WHERE
    bonus < 1000 OR bonus IS NULL
;
```
