

## Solution
---
#### Approach 1: Dynamic Programming

**Intuition**

When writing the permutation `P = P_0, P_1, ..., P_N` from left to right, we only care about the relative rank of the last element placed.  For example, if `N = 5` (so that we have elements `{0, 1, 2, 3, 4, 5}`), and our permutation starts `2, 3, 4`, then it is similar to a situation where we have placed `?, ?, 2` and the remaining elements are `{0, 1, 3}`, in terms of how many possibilities there are to place the remaining elements in a valid way.

To this end, let `dp(i, j)` be the number of ways to place every number up to and inlcuding `P_i`, such that `P_i` when placed had relative rank `j`.  (Namely, there are `j` remaining numbers less than `P_i`.)

**Algorithm**

When placing `P_i` following a decreasing instruction `S[i-1] == 'D'`, we want `P_{i-1}` to have a higher value.  When placing `P_i` following an increasing instruction, we want `P_{i-1}` to have a lower value.  It is relatively easy to deduce the recursion from this fact.


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



**Optimization**

Actually, we can do better than this.  For any given `i`, let's look at how the sum of `D_k = dp(i-1, k)` is queried.  Assuming `S[i-1] == 'I'`, we query `D_0, D_0 + D_1, D_0 + D_1 + D_2, ...` etc.  The case for `S[i-1] == 'D'` is similar.

Thus, we don't need to query the sum every time.  Instead, we could use (for `S[i-1] == 'I'`) the fact that `dp(i, j) = dp(i, j-1) + dp(i-1, j-1)`.  For `S[i-1] == 'D'`, we have the similar fact that `dp(i, j) = dp(i, j+1) + dp(i-1, j)`.  

These two facts make the work done for each state of `dp` have $$O(1)$$ (amortized) complexity, leading to a total time complexity of $$O(N^2)$$ for this solution.


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

* Time Complexity:  $$O(N^3)$$, where $$N$$ is the length of `S`, or $$O(N^2)$$ with the optimized version.

* Space Complexity:  $$O(N^2)$$.
<br />
<br />


---
#### Approach 2: Divide and Conquer

**Intuition**

Let's place the zero of the permutation first.  It either goes between a `'DI'` part of the sequence, or it could go on the ends (the left end if it starts with `'I'`, and the right end if it ends in `'D'`.)  Afterwards, this splits the problem into two disjoint subproblems that we can solve with similar logic.

**Algorithm**

Let `dp(i, j)` be the number of valid permutations (of `n = j-i+2` total integers from `0` to `n-1`) corresponding to the DI sequence `S[i], S[i+1], ..., S[j]`.  If we can successfully place a zero between `S[k-1]` and `S[k]`, then there are two disjoint problems `S[i], ..., S[k-2]` and `S[k+1], ..., S[j]`.

To count the number of valid permutations in this case, we should choose `k-i` elements from `n-1` (`n` total integers, minus the zero) to put in the left group; then the answer is this, times the number of ways to arrange the left group [`dp(i, k-2)`], times the number of ways to arrange the right group [`dp(k+1, j)`].


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

* Time Complexity:  $$O(N^2)$$, where $$N$$ is the length of `S`.

* Space Complexity:  $$O(N^2)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
