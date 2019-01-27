

## Solution
---
#### Approach 1: Depth-First Search

**Intuition**

It's natural to try to assign everyone to a group.  Let's say people in the first group are red, and people in the second group are blue.

If the first person is red, anyone disliked by this person must be blue.  Then, anyone disliked by a blue person is red, then anyone disliked by a red person is blue, and so on.

If at any point there is a conflict, the task is impossible, as every step logically follows from the first step.  If there isn't a conflict, then the coloring was valid, so the answer would be `true`.

**Algorithm**

Consider the graph on `N` people formed by the given "dislike" edges.  We want to check that each connected component of this graph is bipartite.

For each connected component, we can check whether it is bipartite by just trying to coloring it with two colors.  How to do this is as follows: color any node red, then all of it's neighbors blue, then all of those neighbors red, and so on.  If we ever color a red node blue (or a blue node red), then we've reached a conflict.


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

* Time Complexity:  $$O(N + E)$$, where $$E$$ is the length of `dislikes`.

* Space Complexity:  $$O(N + E)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
