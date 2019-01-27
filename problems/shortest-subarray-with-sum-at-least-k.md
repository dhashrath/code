

## Solution
---
#### Approach 1: Sliding Window

**Intuition**

We can rephrase this as a problem about the prefix sums of `A`.  Let `P[i] = A[0] + A[1] + ... + A[i-1]`.  We want the smallest `y-x` such that `y > x` and `P[y] - P[x] >= K`.

Motivated by that equation, let `opt(y)` be the largest `x` such that `P[x] <= P[y] - K`.  We need two key observations:

* If `x1 < x2` and `P[x2] <= P[x1]`, then `opt(y)` can never be `x1`, as if `P[x1] <= P[y] - K`, then `P[x2] <= P[x1] <= P[y] - K` but `y - x2` is smaller.  This implies that our candidates `x` for `opt(y)` will have increasing values of `P[x]`.

* If `opt(y1) = x`, then we do not need to consider this `x` again.  For if we find some `y2 > y1` with `opt(y2) = x`, then it represents an answer of `y2 - x` which is worse (larger) than `y1 - x`.

**Algorithm**

Maintain a "monoqueue" of indices of `P`: a deque of indices `x_0, x_1, ...` such that `P[x_0], P[x_1], ...` is increasing.

When adding a new index `y`, we'll pop `x_i` from the end of the deque so that `P[x_0], P[x_1], ..., P[y]` will be increasing.

If `P[y] >= P[x_0] + K`, then (as previously described), we don't need to consider this `x_0` again, and we can pop it from the front of the deque.


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
