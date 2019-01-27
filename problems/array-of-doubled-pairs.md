

## Solution
---
#### Approach 1: Greedy

**Intuition**

If `x` is currently the array element with the least absolute value, it must pair with `2*x`, as there does not exist any other `x/2` to pair with it.

**Algorithm**

Let's try to (virtually) "write" the final reordered array.

Let's check elements in order of absolute value.  When we check an element `x` and it isn't used, it must pair with `2*x`.  We will attempt to write `x, 2x` - if we can't, then the answer is `false`.  If we write everything, the answer is `true`.

To keep track of what we have not yet written, we will store it in a `count`.


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

* Space Complexity:  $$O(N)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
