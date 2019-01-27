

## Solution

---
#### Approach #1: Recursion [Accepted]

**Intuition**

Let `trim(node)` be the desired answer for the subtree at that node.  We can construct the answer recursively.

**Algorithm**

When $$\text{node.val > R}$$, we know that the trimmed binary tree must occur to the left of the node. Similarly, when $$\text{node.val < L}$$, the trimmed binary tree occurs to the right of the node. Otherwise, we will trim both sides of the tree.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the total number of nodes in the given tree.  We visit each node at most once.

* Space Complexity: $$O(N)$$.  Even though we don't explicitly use any additional memory, the call stack of our recursion could be as large as the number of nodes in the worst case.

---
Analysis written by: [@awice](https://leetcode.com/awice)
