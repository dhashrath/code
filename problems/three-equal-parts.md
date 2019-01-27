

## Solution
---
#### Approach 1: Equal Ones

**Intuition**

Each part has to have the same number of ones in their representation.  The algorithm given below is the natural continuation of this idea.

**Algorithm**

Say `S` is the number of ones in `A`.  Since every part has the same number of ones, they all should have `T = S / 3` ones.

If `S` isn't divisible by 3, the task is impossible.

We can find the position of the 1st, T-th, T+1-th, 2T-th, 2T+1-th, and 3T-th one.  The positions of these ones form 3 intervals: `[i1, j1], [i2, j2], [i3, j3]`.  (If there are only 3 ones, then the intervals are each length 1.)

Between them, there may be some number of zeros.  The zeros after `j3` must be included in each part: say there are `z` of them `(z = S.length - j3)`.

So the first part, `[i1, j1]`, is now `[i1, j1+z]`.  Similarly, the second part, `[i2, j2]`, is now `[i2, j2+z]`.

If all this is actually possible, then the final answer is `[j1+z, j2+z+1]`.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `S`.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
