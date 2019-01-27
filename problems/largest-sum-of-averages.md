

---
#### Approach #1: Dynamic Programming [Accepted]

**Intuition**

The best score partitioning `A[i:]` into at most `K` parts depends on answers to paritioning `A[j:]` (`j > i`) into less parts.  We can use dynamic programming as the states form a directed acyclic graph.

**Algorithm**

Let `dp(i, k)` be the best score partioning `A[i:]` into at most `K` parts.

If the first group we partition `A[i:]` into ends before `j`, then our candidate partition has score `average(i, j) + dp(j, k-1))`, where `average(i, j) = (A[i] + A[i+1] + ... + A[j-1]) / (j - i)` (floating point division).  We take the highest score of these, keeping in mind we don't necessarily need to partition - `dp(i, k)` can also be just `average(i, N)`.

In total, our recursion in the general case is `dp(i, k) = max(average(i, N), max_{j > i}(average(i, j) + dp(j, k-1)))`.

We can calculate `average` a little bit faster by remembering prefix sums.  If `P[x+1] = A[0] + A[1] + ... + A[x]`, then `average(i, j) = (P[j] - P[i]) / (j - i)`.

Our implementation showcases a "bottom-up" style of dp.  Here at loop number `k` in our outer-most loop, `dp[i]` represents `dp(i, k)` from the discussion above, and we are calculating the next layer `dp(i, k+1)`.  The end of our second loop `for i = 0..N-1` represents finishing the calculation of the correct value for `dp(i, t)`, and the inner-most loop performs the calculation `max_{j > i}(average(i, j) + dp(j, k))`.


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

* Time Complexity:  $$O(K * N^2)$$, where $$N$$ is the length of `A`.

* Space Complexity: $$O(N)$$, the size of `dp`.

---

Analysis written by: [@awice](https://leetcode.com/awice).
