

## Solution
---
#### Approach 1: Next Array

**Intuition**

Instead of checking whether `all(L <= R for L in left for R in right)`, let's check whether `max(left) <= min(right)`.

**Algorithm**

Let's try to find `max(left)` for subarrays `left = A[:1], left = A[:2], left =  A[:3], ...` etc.  Specifically, `maxleft[i]` will be the maximum of subarray `A[:i]`.  They are related to each other: `max(A[:4]) = max(max(A[:3]), A[3])`, so `maxleft[4] = max(maxleft[3], A[3])`.

Similarly, `min(right)` for every possible `right` can be found in linear time.

After we have a way to query `max(left)` and `min(right)` quickly, the solution is straightforward.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `A`.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
