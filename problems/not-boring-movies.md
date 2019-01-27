

## Solution
---
#### Approach: Using `MOD()` function [Accepted]

**Algorithm**

We can use the `mod(id,2)=1` to determine the odd id, and then add a `description != 'boring'` should address this problem.

**MySQL**

```sql
select *
from cinema
where mod(id, 2) = 1 and description != 'boring'
order by rating DESC
;
```