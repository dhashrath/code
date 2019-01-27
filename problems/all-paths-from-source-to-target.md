

---
#### Approach #1: Recursion [Accepted]

**Intuition**

Since the graph is a directed, acyclic graph, any path from `A` to `B` is going to be composed of `A` plus a path from any neighbor of `A` to `B`.  We can use a recursion to return the answer.

**Algorithm**

Let `N` be the number of nodes in the graph.  If we are at node `N-1`, the answer is just the path `{N-1}`.  Otherwise, if we are at node `node`, the answer is `{node} + {path from nei to N-1}` for each neighbor `nei` of `node`.  This is a natural setting to use a recursion to form the answer.


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

* Time Complexity:  $$O(2^N N^2)$$.  We can have exponentially many paths, and for each such path, our prepending operation `path.add(0, node)` will be $$O(N^2)$$.

* Space Complexity: $$O(2^N N)$$, the size of the output dominating the final space complexity.

---

Analysis written by: [@awice](https://leetcode.com/awice).
