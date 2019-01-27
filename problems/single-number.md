

## Solution

---

#### Approach 1: List operation

**Algorithm**

1. Iterate over all the elements in $$\text{nums}$$
2. If some number in $$\text{nums}$$ is new to array, append it
3. If some number is already in the array, remove it


```java
public class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length < 2)
            return nums.length;
        int[] up = new int[nums.length];
        int[] down = new int[nums.length];
        for (int i = 1; i < nums.length; i++) {
            for(int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    up[i] = Math.max(up[i],down[j] + 1);
                } else if (nums[i] < nums[j]) {
                    down[i] = Math.max(down[i],up[j] + 1);
                }
            }
        }
        return 1 + Math.max(down[nums.length - 1], up[nums.length - 1]);
    }
}```


**Complexity Analysis**

* Time complexity : $$O(n^2)$$. We iterate through $$\text{nums}$$, taking $$O(n)$$ time. We search the whole list to find whether there is duplicate number, taking $$O(n)$$ time. Because search is in the `for` loop, so we have to multiply both time complexities which is $$O(n^2)$$.

* Space complexity : $$O(n)$$.  We need a list of size $$n$$ to contain elements in $$\text{nums}$$.
<br />
<br />
---
#### Approach 2: Hash Table

**Algorithm**

We use hash table to avoid the $$O(n)$$ time required for searching the elements.

1. Iterate through all elements in $$\text{nums}$$
2. Try if $$hash\_table$$ has the key for `pop`
3. If not, set up key/value pair
4. In the end, there is only one element in $$hash\_table$$, so use `popitem` to get it


```java
public class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length < 2)
            return nums.length;
        int[] up = new int[nums.length];
        int[] down = new int[nums.length];
        for (int i = 1; i < nums.length; i++) {
            for(int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    up[i] = Math.max(up[i],down[j] + 1);
                } else if (nums[i] < nums[j]) {
                    down[i] = Math.max(down[i],up[j] + 1);
                }
            }
        }
        return 1 + Math.max(down[nums.length - 1], up[nums.length - 1]);
    }
}```


**Complexity Analysis**

* Time complexity : $$O(n \cdot 1) = O(n)$$.  Time complexity of `for` loop is $$O(n)$$. Time complexity of hash table(dictionary in python) operation `pop` is $$O(1)$$.

* Space complexity : $$O(n)$$. The space required by $$hash\_table$$ is equal to the number of elements in $$\text{nums}$$.
<br />
<br />
---
#### Approach 3: Math

**Concept**

$$2 * (a + b + c) - (a + a + b + b + c) = c$$


```java
public class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length < 2)
            return nums.length;
        int[] up = new int[nums.length];
        int[] down = new int[nums.length];
        for (int i = 1; i < nums.length; i++) {
            for(int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    up[i] = Math.max(up[i],down[j] + 1);
                } else if (nums[i] < nums[j]) {
                    down[i] = Math.max(down[i],up[j] + 1);
                }
            }
        }
        return 1 + Math.max(down[nums.length - 1], up[nums.length - 1]);
    }
}```


**Complexity Analysis**

* Time complexity : $$O(n + n) = O(n)$$. `sum` will call `next` to iterate through $$\text{nums}$$.
We can see it as `sum(list(i, for i in nums))` which means the time complexity is $$O(n)$$ because of the number of elements($$n$$) in $$\text{nums}$$.

* Space complexity : $$O(n + n) = O(n)$$. `set` needs space for the elements in `nums`
<br />
<br />
---
#### Approach 4: Bit Manipulation

**Concept**

- If we take XOR of zero and some bit, it will return that bit
    - $$a \oplus 0 = a$$
- If we take XOR of two same bits, it will return 0
    - $$a \oplus a = 0$$
- $$a \oplus b \oplus a = (a \oplus a) \oplus b = 0 \oplus b = b$$

So we can XOR all bits together to find the unique number.


```java
public class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length < 2)
            return nums.length;
        int[] up = new int[nums.length];
        int[] down = new int[nums.length];
        for (int i = 1; i < nums.length; i++) {
            for(int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    up[i] = Math.max(up[i],down[j] + 1);
                } else if (nums[i] < nums[j]) {
                    down[i] = Math.max(down[i],up[j] + 1);
                }
            }
        }
        return 1 + Math.max(down[nums.length - 1], up[nums.length - 1]);
    }
}```


**Complexity Analysis**

* Time complexity : $$O(n)$$.  We only iterate through $$\text{nums}$$, so the time complexity is the number of elements in $$\text{nums}$$.

* Space complexity : $$O(1)$$.