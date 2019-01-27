

## Solution
---
#### Approach: Using `round` and `ifnull` [Accepted]

**Intuition**

Count the accepted requests and then divides it by the number of all requests.

**Algorithm**

To get the distinct number of accepted requests, we can query from the **request_accepted** table.
```sql
select count(*) from (select distinct requester_id, accepter_id from request_accepted;
```

With the same technique, we can have the total number of requests from the **friend_request** table:
```sql
select count(*) from (select distinct sender_id, send_to_id from friend_request;
```

At last, divide these two numbers and [`round`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_round) it to a scale of 2 decimal places to get the required acceptance rate.

Wait! The divisor (total number of requests) could be '0' if the table **friend_request** is empty. So, we have to utilize  [`ifnull`](https://dev.mysql.com/doc/refman/5.7/en/control-flow-functions.html#function_ifnull) to deal with this special case.

**MySQL**

```sql
select
round(
    ifnull(
    (select count(*) from (select distinct requester_id, accepter_id from request_accepted) as A)
    /
    (select count(*) from (select distinct sender_id, send_to_id from friend_request) as B),
    0)
, 2) as accept_rate;
```
