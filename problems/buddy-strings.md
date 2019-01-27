

## Solution
---
#### Approach 1: Enumerate Cases

**Intuition**

Let's say `i` is *matched* if `A[i] == B[i]`, otherwise `i` is *unmatched*.  A buddy string has almost all matches, because a swap only affects two indices.

If swapping `A[i]` and `A[j]` would demonstrate that `A` and `B` are buddy strings, then `A[i] == B[j]` and `A[j] == B[i]`.  That means among the four free variables `A[i], A[j], B[i], B[j]`, there are only two cases: either `A[i] == A[j]` or not.

**Algorithm**

Let's work through the cases.

In the case `A[i] == A[j] == B[i] == B[j]`, then the strings `A` and `B` are equal.  So if `A == B`, we should check each index `i` for two matches with the same value.

In the case `A[i] == B[j], A[j] == B[i], (A[i] != A[j])`, the rest of the indices match.  So if `A` and `B` have only two unmatched indices (say `i` and `j`), we should check that the equalities `A[i] == B[j]` and `A[j] == B[i]` hold.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `A` and `B`.

* Space Complexity:  $$O(1)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
