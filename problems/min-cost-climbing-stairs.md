

#### Approach #1: Dynamic Programming [Accepted]

**Intuition**

There is a clear recursion available: the final cost `f[i]` to climb the staircase from some step `i` is `f[i] = cost[i] + min(f[i+1], f[i+2])`.  This motivates *dynamic programming*.

**Algorithm**

Let's evaluate `f` backwards in order.  That way, when we are deciding what `f[i]` will be, we've already figured out `f[i+1]` and `f[i+2]`.

We can do even better than that.  At the `i`-th step, let `f1, f2` be the old value of `f[i+1]`, `f[i+2]`, and update them to be the new values `f[i], f[i+1]`.  We keep these updated as we iterate through `i` backwards.  At the end, we want `min(f1, f2)`.


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

* Time Complexity: $$O(N)$$ where $$N$$ is the length of `cost`.

* Space Complexity: $$O(1)$$, the space used by `f1, f2`.

---

Analysis written by: [@awice](https://leetcode.com/awice).
