

## Solution
---
#### Approach I: Using sub-query and `SUM()` function [Accepted]

**Intuition**

Calculate the answered times / show times for each question.

**Algorithm**

First, we can use `SUM()` to get the total number of answered times as well as the show times for each question using a sub-query as below.

```sql
SELECT
    question_id,
    SUM(CASE
        WHEN action = 'answer' THEN 1
        ELSE 0
    END) AS num_answer,
    SUM(CASE
        WHEN action = 'show' THEN 1
        ELSE 0
    END) AS num_show
FROM
    survey_log
GROUP BY question_id
;
```

```
| question_id | num_answer | num_show |
|-------------|------------|----------|
| 285         | 1          | 1        |
| 369         | 0          | 1        |
```

Then we can calculate the answer rate by its definition.

**MySQL**

```sql
SELECT question_id as survey_log
FROM
(
	SELECT question_id,
         SUM(case when action="answer" THEN 1 ELSE 0 END) as num_answer,
        SUM(case when action="show" THEN 1 ELSE 0 END) as num_show,    
	FROM survey_log
	GROUP BY question_id
) as tbl
ORDER BY (num_answer / num_show) DESC
LIMIT 1
```

#### Approach II: Using sub-query and `COUNT(IF...)` function [Accepted]

**Algorithm**

This solution is very straight forward: use the `COUNT()` function to sum the answer and show time combining with the `IF()` function.

**MySQL**
```sql
SELECT 
    question_id AS 'survey_log'
FROM
    survey_log
GROUP BY question_id
ORDER BY COUNT(answer_id) / COUNT(IF(action = 'show', 1, 0)) DESC
LIMIT 1;
```
