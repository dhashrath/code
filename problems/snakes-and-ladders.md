

## Solution
---
#### Approach 1: Breadth-First Search

**Intuition**

As we are looking for a shortest path, a breadth-first search is ideal.  The main difficulty is to handle enumerating all possible moves from each square.

**Algorithm**

Suppose we are on a square with number `s`.  We would like to know all final destinations with number `s2` after making one move.

This requires knowing the coordinates `get(s2)` of square `s2`.  This is a small puzzle in itself: we know that the row changes every `N` squares, and so is only based on `quot = (s2-1) / N`; also the column is only based on `rem = (s2-1) % N` and what row we are on (forwards or backwards.)

From there, we perform a breadth first search, where the nodes are the square numbers `s`.


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

* Time Complexity:  $$O(N^2)$$, where $$N$$ is the length of the `board`.

* Space Complexity:  $$O(N^2)$$.
<br />
<br />


---

Analysis written by: [@awice](https://leetcode.com/awice).
