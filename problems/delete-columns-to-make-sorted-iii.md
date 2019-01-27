

## Solution
---
#### Approach 1: Dynamic Programming

**Intuition and Algorithm**

This is a tricky problem that is hard to build an intuition about.

First, lets try to find the number of columns to keep, instead of the number to delete.  At the end, we can subtract to find the desired answer.

Now, let's say we must keep the first column `C`.  The next column `D` we keep must have all rows lexicographically sorted (ie. `C[i] <= D[i]`), and we can say that we have deleted all columns between `C` and `D`.

Now, we can use dynamic programming to solve the problem in this manner.  Let `dp[k]` be the number of columns that are kept in answering the question for input `[row[k:] for row in A]`.  The above gives a simple recursion for `dp[k]`.


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

* Time Complexity:  $$O(N * W^2)$$, where $$N$$ is the length of `A`, and $$W$$ is the length of each word in `A`.

* Space Complexity:  $$O(W)$$.
<br />
<br />


---
Analysis written by: [@awice](https://leetcode.com/awice).
