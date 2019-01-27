

---
#### Approach #1: Recursion [Accepted]

**Intuition**

Prune children of the tree recursively.  The only decisions at each node are whether to prune the left child or the right child.

**Algorithm**

We'll use a function `containsOne(node)` that does two things: it tells us whether the subtree at this `node` contains a `1`, and it also prunes all subtrees not containing `1`.

If for example, `node.left` does not contain a one, then we should prune it via `node.left = null`.

Also, the parent needs to be checked.  If for example the tree is a single node `0`, the answer is an empty tree.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the number of nodes in the tree.  We process each node once.

* Space Complexity: $$O(H)$$, where $$H$$ is the height of the tree.  This represents the size of the implicit call stack in our recursion.

---

Analysis written by: [@awice](https://leetcode.com/awice).
