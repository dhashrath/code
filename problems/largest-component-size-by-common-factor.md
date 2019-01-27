

## Solution
---
#### Approach 1: Union-Find

We will skip the explanation of how a DSU structure is implemented.  Please refer to [https://leetcode.com/problems/redundant-connection/solution/](https://leetcode.com/problems/redundant-connection/solution/) for a tutorial on DSU.

**Intuition**

Let $$W = \max(A[i])$$, and $$R = \sqrt{W}$$.  For each value $$A[i]$$, there is at most one prime factor $$p \geq R$$ dividing $$A[i]$$.  Let's call $$A[i]$$'s "big prime" this $$p$$, if it exists.

This means that there are at most $$R + A\text{.length}$$ unique prime divisors of elements in $$A$$:  the big primes correspond to a maximum of $$A\text{.length}$$ values, and the small primes are all less than $$R$$, so there's at most $$R$$ of them too.

**Algorithm**

Factor each $$A[i]$$ into prime factors, and index every occurrence of these primes.  (To save time, we can use a sieve.  Please see this article's comments for more details.)

Then, use a union-find structure to union together any prime factors that came from the same $$A[i]$$.

Finally, we can count the size of each component, by inspecting and counting the id of the component each $$A[i]$$ belongs to.


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

* Time Complexity:  $$O(N\sqrt{W})$$ where $$N$$ is the length of `A`, and $$W = \max(A[i])$$.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
