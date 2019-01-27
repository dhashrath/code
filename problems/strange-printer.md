

#### Approach #1: Dynamic Programming [Accepted]

**Intuition**

It is natural to consider letting `dp(i, j)` be the answer for printing `S[i], S[i+1], ..., S[j]`, but proceeding from here is difficult.  We need the following sequence of deductions:

* Whatever turn creates the final print of `S[i]` might as well be the first turn, and also there might as well only be one print, since any later prints on interval `[i, k]` could just be on `[i+1, k]`.

* Say the first print is on `[i, k]`.  We can assume `S[i] == S[k]`, because if it wasn't, we could print up to the last occurrence of `S[i]` in `[i, k]` for the same result.

* When correctly printing everything in `[i, k]` (with `S[i] == S[k]`), it will take the same amount of steps as correctly printing everything in `[i, k-1]`.  This is because if `S[i]` and `S[k]` get completed in separate steps, we might as well print them first in one step instead.

**Algorithm**

With the above deductions, the algorithm is straightforward.

To compute a recursion for `dp(i, j)`, for every `i <= k <= j` with `S[i] == S[k]`, we have some candidate answer `dp(i, k-1) + dp(k+1, j)`, and we take the minimum of these candidates.  Of course, when `k = i`, the candidate is just `1 + dp(i+1, j)`.

To avoid repeating work, we memoize our intermediate answers `dp(i, j)`.


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

* Time Complexity: $$O(N^3)$$ where $$N$$ is the length of `s`.  For each of $$O(N^2)$$ possible states representing a subarray of `s`, we perform $$O(N)$$ work iterating through `k`.

* Space Complexity: $$O(N^2)$$, the size of our `memo`.

---

Analysis written by: [@awice](https://leetcode.com/awice).
