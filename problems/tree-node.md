

## Solution
---

#### Approach I: Using `UNION` [Accepted]

**Intuition**

We can print the node type by judging every record by its definition in this table.
* Root: it does not have a parent node at all
* Inner: it is the parent node of some nodes, and it has a not NULL parent itself.
* Leaf: rest of the cases other than above two

**Algorithm**

By transiting the node type definition, we can have the following code.

For the root node, it does not have a parent.
```sql
SELECT
    id, 'Root' AS Type
FROM
    tree
WHERE
    p_id IS NULL
```

For the leaf nodes, they do not have any children, and it has a parent.
```sql
SELECT
    id, 'Leaf' AS Type
FROM
    tree
WHERE
    id NOT IN (SELECT DISTINCT
            p_id
        FROM
            tree
        WHERE
            p_id IS NOT NULL)
        AND p_id IS NOT NULL
```

For the inner nodes, they have have some children and a parent.
```sql
SELECT
    id, 'Inner' AS Type
FROM
    tree
WHERE
    id IN (SELECT DISTINCT
            p_id
        FROM
            tree
        WHERE
            p_id IS NOT NULL)
        AND p_id IS NOT NULL
```
So, one solution to the problem is to combine these cases together using `UNION`.

**MySQL**

```sql
SELECT
    id, 'Root' AS Type
FROM
    tree
WHERE
    p_id IS NULL

UNION

SELECT
    id, 'Leaf' AS Type
FROM
    tree
WHERE
    id NOT IN (SELECT DISTINCT
            p_id
        FROM
            tree
        WHERE
            p_id IS NOT NULL)
        AND p_id IS NOT NULL

UNION

SELECT
    id, 'Inner' AS Type
FROM
    tree
WHERE
    id IN (SELECT DISTINCT
            p_id
        FROM
            tree
        WHERE
            p_id IS NOT NULL)
        AND p_id IS NOT NULL
ORDER BY id;
```

#### Approach II: Using flow control statement `CASE` [Accepted]

**Algorithm**

The idea is similar with the above solution but the code is simpler by utilizing the flow control statements, which is effective to output differently based on different input values. In this case, we can use [`CASE`](https://dev.mysql.com/doc/refman/5.7/en/case.html) statement.

**MySQL**

```sql
SELECT
    id AS `Id`,
    CASE
        WHEN tree.id = (SELECT atree.id FROM tree atree WHERE atree.p_id IS NULL)
          THEN 'Root'
        WHEN tree.id IN (SELECT atree.p_id FROM tree atree)
          THEN 'Inner'
        ELSE 'Leaf'
    END AS Type
FROM
    tree
ORDER BY `Id`
;
```
>MySQL provides different flow control statements besides `CASE`. You can try to rewrite the slution above using [`IF`](https://dev.mysql.com/doc/refman/5.7/en/control-flow-functions.html#function_if) flow control statement.

#### Approach III: Using `IF` function [Accepted]

**Algorithm**

Also, we can use a single [`IF`](https://dev.mysql.com/doc/refman/5.7/en/control-flow-functions.html#function_if) function instead of the complex flow control statements.

**MySQL**

```sql
SELECT
    atree.id,
    IF(ISNULL(atree.p_id),
        'Root',
        IF(atree.id IN (SELECT p_id FROM tree), 'Inner','Leaf')) Type
FROM
    tree atree
ORDER BY atree.id
```
>Note: This solution was inspired by [@richarddia](https://discuss.leetcode.com/user/richarddia)
