#### Biggest Single Number

```mysql
SELECT
    num
FROM
    `number`
GROUP BY num
HAVING COUNT(num) = 1;```


```mysql
SELECT
    MAX(num) AS num
FROM
    (SELECT
        num
    FROM
        number
    GROUP BY num
    HAVING COUNT(num) = 1) AS t
;```

