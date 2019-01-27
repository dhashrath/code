

## Solution
---
#### Approach 1: Sort

**Intuition**

Without loss of generality, say the sidelengths of the triangle are $$a \leq b \leq c$$.  The necessary and sufficient condition for these lengths to form a triangle of non-zero area is $$a + b > c$$.

Say we knew $$c$$ already.  There is no reason not to choose the largest possible $$a$$ and $$b$$ from the array.  If $$a + b > c$$, then it forms a triangle, otherwise it doesn't.

**Algorithm**

This leads to a simple algorithm:  Sort the array.  For any $$c$$ in the array, we choose the largest possible $$a \leq b \leq c$$:  these are just the two values adjacent to $$c$$.  If this forms a triangle, we return the answer.


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

* Time Complexity:  $$O(N \log N)$$, where $$N$$ is the length of `A`.

* Space Complexity:  $$O(1)$$.
<br />
<br />


---
Analysis written by: [@awice](https://leetcode.com/awice).
