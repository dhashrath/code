

#### Approach #1: Recursion [Accepted]

**Intuition**

We can think of any path (of nodes with the same values) as up to two arrows extending from it's root.

Specifically, the *root* of a path will be the unique node such that the parent of that node does not appear in the path, and an *arrow* will be a path where the root only has one child node in the path.

Then, for each node, we want to know what is the longest possible arrow extending left, and the longest possible arrow extending right?  We can solve this using recursion.

**Algorithm**

Let `arrow_length(node)` be the length of the longest arrow that extends from the `node`.  That will be `1 + arrow_length(node.left)` if `node.left` exists and has the same value as `node`.  Similarly for the `node.right` case.

While we are computing arrow lengths, each candidate answer will be the sum of the arrows in both directions from that node.  We record these candidate answers and return the best one.


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

* Time Complexity: $$O(N)$$, where $$N$$ is the number of nodes in the tree.  We process every node once.

* Space Complexity: $$O(H)$$, where $$H$$ is the height of the tree.  Our recursive call stack could be up to $$H$$ layers deep.

---

Analysis written by: [@awice](https://leetcode.com/awice)
