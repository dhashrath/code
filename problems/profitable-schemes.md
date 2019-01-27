

## Solution
---
#### Approach 1: Dynamic Programming

**Intuition**

We don't care about the profit of the scheme if it is $$\geq P$$, because it surely will be over the threshold of profitability required.  Similarly, we don't care about the number of people required in the scheme if it is $$> G$$, since we know the scheme will be too big for the gang to execute.

As a result, the bounds are small enough to use dynamic programming.  Let's keep track of `cur[p][g]`, the number of schemes with profitability $$p$$ and requiring $$g$$ gang members: except we'll say (without changing the answer) that all schemes that profit *at least* `P` dollars will instead profit *exactly* `P` dollars.

**Algorithm**

Keeping track of `cur[p][g]` as defined above, let's understand how it changes as we consider 1 extra crime that will profit `p0` and require `g0` gang members.  We will put the updated counts into `cur2`.

For each possible scheme with profit `p1` and group size `g1`, that scheme plus the extra crime (`p0, g0`) being considered, has a profit of `p2 = min(p1 + p0, P)`, and uses a group size of `g2 = g1 + g0`.


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

* Time Complexity:  $$O(N * P * G)$$, where $$N$$ is the number of crimes available to the gang.

* Space Complexity:  $$O(P * G)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
