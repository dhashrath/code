

## Solution
---
#### Approach 1: Depth First Search

**Intuition**

If the leaf of a tree has 0 coins (an excess of -1 from what it needs), then we should push a coin from its parent onto the leaf.  If it has say, 4 coins (an excess of 3), then we should push 3 coins off the leaf.  In total, the number of moves from that leaf to or from its parent is `excess = Math.abs(num_coins - 1)`.  Afterwards, we never have to consider this leaf again in the rest of our calculation.

**Algorithm**

We can use the above fact to build our answer.  Let `dfs(node)` be the *excess* number of coins in the subtree at or below this `node`: namely, the number of coins in the subtree, minus the number of nodes in the subtree.  Then, the number of moves we make from this node to and from its children is `abs(dfs(node.left)) + abs(dfs(node.right))`.  After, we have an excess of `node.val + dfs(node.left) + dfs(node.right) - 1` coins at this node.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the number of nodes in the tree.

* Space Complexity:  $$O(H)$$, where $$H$$ is the height of the tree.
<br />
<br />


---
Analysis written by: [@awice](https://leetcode.com/awice).
