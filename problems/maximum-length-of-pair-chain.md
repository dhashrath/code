

---
#### Approach #1: Dynamic Programming [Accepted]

**Intuition**

If a chain of length `k` ends at some `pairs[i]`, and `pairs[i][1] < pairs[j][0]`, we can extend this chain to a chain of length `k+1`.

**Algorithm**

Sort the pairs by first coordinate, and let `dp[i]` be the length of the longest chain ending at `pairs[i]`.  When `i < j` and `pairs[i][1] < pairs[j][0]`, we can extend the chain, and so we have the candidate answer `dp[j] = max(dp[j], dp[i] + 1)`.


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

* Time Complexity: $$O(N^2)$$ where $$N$$ is the length of `pairs`.  There are two for loops, and $$N^2$$ dominates the sorting step.

* Space Complexity: $$O(N)$$ for sorting and to store `dp`.

---
#### Approach #2: Greedy [Accepted]

**Intuition**

We can greedily add to our chain.  Choosing the next addition to be the one with the lowest second coordinate is at least better than a choice with a larger second coordinate.

**Algorithm**

Consider the pairs in increasing order of their *second* coordinate.  We'll try to add them to our chain.  If we can, by the above argument we know that it is correct to do so.


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

* Time Complexity: $$O(N \log N)$$ where $$N$$ is the length of `S`.  The complexity comes from the sorting step, but the rest of the solution does linear work.

* Space Complexity: $$O(N)$$.  The additional space complexity of storing `cur` and `ans`, but sorting uses $$O(N)$$ space.  Depending on the implementation of the language used, sorting can sometimes use less space.

---

Analysis written by: [@awice](https://leetcode.com/awice).
