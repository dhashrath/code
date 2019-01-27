

## Solution
---
#### Approach: Using `avg()` and `case...when...` [Accepted]

**Intuition**

Solve this problem by 3 steps as below.

**Algorithm**

1.Calculate the company's average salary in every month
MySQL has the built-in function avg() to get the average of a list of numbers. Also, we need to format the *pay_date* column for future use.

```sql
select avg(amount) as company_avg,  date_format(pay_date, '%Y-%m') as pay_month
from salary
group by date_format(pay_date, '%Y-%m')
;
```

| company_avg | pay_month |
|-------------|-----------|
| 7000.0000   | 2017-02   |
| 8333.3333   | 2017-03   |

2.Calculate the each department's average salary in every month
To do this, we need to join the **salary** table with the **employee** table using condition `salary.employee_id = employee.id`.

```sql
select department_id, avg(amount) as department_avg, date_format(pay_date, '%Y-%m') as pay_month
from salary
join employee on salary.employee_id = employee.employee_id
group by department_id, pay_month
;
```

| department_id | department_avg | pay_month |
|---------------|----------------|-----------|
| 1             | 7000.0000      | 2017-02   |
| 1             | 9000.0000      | 2017-03   |
| 2             | 7000.0000      | 2017-02   |
| 2             | 8000.0000      | 2017-03   |

3.Compare the previous numbers and display the result
This step might be the hardest if you have no idea on how to use MySQL flow control statement [`case...when...`](https://dev.mysql.com/doc/refman/5.7/en/case.html).

MySQL, like other programming languages, also has its flow control. Click [this link](https://dev.mysql.com/doc/refman/5.7/en/flow-control-statements.html) to learn it.

At last, combine the above two query and join them `on department_salary.pay_month = company_salary.pay_month`.

**MySQL**

```sql
select department_salary.pay_month, department_id,
case
  when department_avg>company_avg then 'higher'
  when department_avg<company_avg then 'lower'
  else 'same'
end as comparison
from
(
  select department_id, avg(amount) as department_avg, date_format(pay_date, '%Y-%m') as pay_month
  from salary join employee on salary.employee_id = employee.employee_id
  group by department_id, pay_month
) as department_salary
join
(
  select avg(amount) as company_avg,  date_format(pay_date, '%Y-%m') as pay_month from salary group by date_format(pay_date, '%Y-%m')
) as company_salary
on department_salary.pay_month = company_salary.pay_month
;
```
