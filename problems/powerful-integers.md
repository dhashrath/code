

## Solution
---
#### Approach 1: Brute Force

**Intuition**

If $$x^i > \text{bound}$$, the sum $$x^i + y^j$$ can't be less than or equal to the bound.  Similarly for $$y^j$$.

Thus, we only have to check for $$0 \leq i, j \leq \log_x(\text{bound}) < 18$$.

We can use a `HashSet` to store all the different values.


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

* Time Complexity:  $$O(\log^2{\text{bound}})$$.

* Space Complexity:  $$O(\log^2{\text{bound}})$$.
<br />
<br />


---
Analysis written by: [@awice](https://leetcode.com/awice).
