

## Solution
---
#### Approach: Union *requester_id* and *accepter_id* [Accepted]

**Algorithm**

Being friends is bidirectional, so if one person accepts a request from another person, both of them will have one more friend.

Thus, we can union column *requester_id* and *accepter_id*, and then count the number of the occurrence of each person.

```mysql
select requester_id as ids from request_accepted
union all
select accepter_id from request_accepted;
```
>Note: Here we should use `union all` instead of `union` because `union all` will keep all the records even the 'duplicated' one.

Taking the sample as an example, the output is:

| ids |
|-----|
| 1   |
| 1   |
| 2   |
| 3   |
| 2   |
| 3   |
| 3   |
| 4   |

Then it will be fairly easy to get the 'ids' with most occurrence using the same technique as mentioned in problem [580. Customer Placing the Largest Number of Orders](https://leetcode.com/problems/count-student-number-in-departments/#/description).

**MySQL**

```mysql
select ids as id, cnt as num
from
(
select ids, count(*) as cnt
   from
   (
        select requester_id as ids from request_accepted
        union all
        select accepter_id from request_accepted
    ) as tbl1
   group by ids
   ) as tbl2
order by cnt desc
limit 1
;
```
