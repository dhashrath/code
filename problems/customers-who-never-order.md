

## Solution
---
#### Approach: Using sub-query and `NOT IN` clause [Accepted]

**Algorithm**

If we have a list of customers who have ever ordered, it will be easy to know who never ordered.

We can use the following code to get such list.

```sql
select customerid from orders;
```

Then, we can use `NOT IN` to query the customers who are not in this list.

**MySQL**

```sql
select customers.name as 'Customers'
from customers
where customers.id not in
(
    select customerid from orders
);
```