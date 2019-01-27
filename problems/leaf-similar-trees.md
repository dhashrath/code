

## Solution
---
#### Approach 1: Depth First Search

**Intuition and Algorithm**

Let's find the leaf value sequence for both given trees.  Afterwards, we can compare them to see if they are equal or not.

To find the leaf value sequence of a tree, we use a depth first search.  Our `dfs` function writes the node's value if it is a leaf, and then recursively explores each child.  This is guaranteed to visit each leaf in left-to-right order, as left-children are fully explored before right-children.


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

* Time Complexity:  $$O(T_1 + T_2)$$, where $$T_1, T_2$$ are the lengths of the given trees.

* Space Complexity:  $$O(T_1 + T_2)$$, the space used in storing the leaf values.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
