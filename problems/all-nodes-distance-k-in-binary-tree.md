

## Solution
---
#### Approach 1: Annotate Parent

**Intuition**

If we know the parent of every node `x`, we know all nodes that are distance `1` from `x`.  We can then perform a breadth first search from the `target` node to find the answer.

**Algorithm**

We first do a depth first search where we annotate every node with information about it's parent.

After, we do a breadth first search to find all nodes a distance `K` from the `target`.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the number of nodes in the given tree.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---
#### Approach 2: Percolate Distance

**Intuition**

From `root`, say the `target` node is at depth `3` in the left branch.  It means that any nodes that are distance `K - 3` in the right branch should be added to the answer.

**Algorithm**

Traverse every `node` with a depth first search `dfs`.  We'll add all nodes `x` to the answer such that `node` is the node on the path from `x` to `target` that is closest to the `root`.

To help us, `dfs(node)` will return the distance from `node` to the `target`.  Then, there are 4 cases:

* If `node == target`, then we should add nodes that are distance `K` in the subtree rooted at `target`.

* If `target` is in the left branch of `node`, say at distance `L+1`, then we should look for nodes that are distance `K - L - 1` in the right branch.

* If `target` is in the right branch of `node`, the algorithm proceeds similarly.

* If `target` isn't in either branch of `node`, then we stop.

In the above algorithm, we make use of the auxillary function `subtree_add(node, dist)` which adds the nodes in the subtree rooted at `node` that are distance `K - dist` from the given `node`.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the number of nodes in the given tree.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
