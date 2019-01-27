

## Solution
---
#### Approach 1: Dynamic Programming

**Intuition**

Let `f(start, n)` be the number of ways to dial an `n` digit number, where the knight starts at square `start`.  We can create a recursion, writing this in terms of `f(x, n-1)`'s.

**Algorithm**

By hand or otherwise, have a way to query what moves are available at each square.  This implies the exact recursion for `f`.  For example, from `1` we can move to `6, 8`, so `f(1, n) = f(6, n-1) + f(8, n-1)`.

After, let's keep track of `dp[start] = f(start, n)`, and update it for each n from `1, 2, ..., N`.

At the end, the answer is `f(0, N) + f(1, N) + ... + f(9, N) = sum(dp)`.


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

* Time Complexity:  $$O(N)$$.

* Space Complexity:  $$O(1)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
