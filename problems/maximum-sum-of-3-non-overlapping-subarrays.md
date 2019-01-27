

#### Approach #1: Ad-Hoc [Accepted]

**Intuition**

It is natural to consider an array `W` of each interval's sum, where each interval is the given length `K`.  To create `W`, we can either use prefix sums, or manage the sum of the interval as a window slides along the array.

From there, we approach the reduced problem: Given some array `W` and an integer `K`, what is the lexicographically smallest tuple of indices `(i, j, k)` with `i + K <= j` and `j + K <= k` that maximizes `W[i] + W[j] + W[k]`?

**Algorithm**

Suppose we fixed `j`.  We would like to know on the intervals $$i \in [0, j-K]$$ and $$k \in [j+K, \text{len}(W)-1]$$, where the largest value of $$W[i]$$ (and respectively $$W[k]$$) occurs first.  (Here, first means the smaller index.)

We can solve these problems with dynamic programming.  For example, if we know that $$i$$ is where the largest value of $$W[i]$$ occurs first on $$[0, 5]$$, then on $$[0, 6]$$ the first occurrence of the largest $$W[i]$$ must be either $$i$$ or $$6$$.  If say, $$6$$ is better, then we set `best = 6`.

At the end, `left[z]` will be the first occurrence of the largest value of `W[i]` on the interval $$i \in [0, z]$$, and `right[z]` will be the same but on the interval $$i \in [z, \text{len}(W) - 1]$$.  This means that for some choice `j`, the candidate answer must be `(left[j-K], j, right[j+K])`.  We take the candidate that produces the maximum `W[i] + W[j] + W[k]`.


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

* Time Complexity: $$O(N)$$, where $$N$$ is the length of the array.  Every loop is bounded in the number of steps by $$N$$, and does $$O(1)$$ work.

* Space complexity:  $$O(N)$$.  `W`, `left`, and `right` all take $$O(N)$$ memory.

---

Analysis written by: [@awice](https://leetcode.com/awice)
