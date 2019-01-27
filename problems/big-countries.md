

## Solution
---
#### Approach I: Using `WHERE` clause and `OR` [Accepted]

**Intuition**

Use `WHERE` clause in SQL to filter these records and get the target countries.

**Algorithm**

According to the definition, a big country meets at least one of the following two conditions:
1. It has an area of bigger than 3 million square km.
2. It has a population of more than 25 million.

So for the first condition, we can use the following code to get the big countries of this type.
```sql
SELECT name, population, area FROM world WHERE area > 3000000
```

In addition, we can use below code to get big countries of more than 25 million people.
```sql
SELECT name, population, area FROM world WHERE population > 25000000
```

As most people may already come into mind, we can use `OR` to combine these two solutions for the two sub-problems together.

**MySQL**

```sql
SELECT
    name, population, area
FROM
    world
WHERE
    area > 3000000 OR population > 25000000
;
```

#### Approach II: Using `WHERE` clause and `UNION` [Accepted]

**Algorithm**

The idea of this approach is the same as the first one. However, we use `UNION` instead of `OR`.

**MySQL**

```sql
SELECT
    name, population, area
FROM
    world
WHERE
    area > 3000000

UNION

SELECT
    name, population, area
FROM
    world
WHERE
    population > 25000000
;
```
>Note: This solution runs a little bit faster than the first one. However, they do not have big difference.