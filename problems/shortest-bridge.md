

## Solution
---
#### Approach 1: Find and Grow

**Intuition**

Conceptually, our method is very straightforward: find both islands, then for one of the islands, keep "growing" it by 1 until we touch the second island.

We can use a depth-first search to find the islands, and a breadth-first search to "grow" one of them.  This leads to a verbose but correct solution.

**Algorithm**

To find both islands, look for a square with a `1` we haven't visited, and dfs to get the component of that region.  Do this twice.  After, we have two components `source` and `target`.

To find the shortest bridge, do a BFS from the nodes `source`.  When we reach any node in `target`, we will have found the shortest distance.

Please see the code for more implementation details.


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

* Time Complexity:  $$O(\mathcal{A})$$, where $$\mathcal{A}$$ is the content of `A`.

* Space Complexity:  $$O(\mathcal{A})$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
