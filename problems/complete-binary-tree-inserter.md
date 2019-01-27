

## Solution
---
#### Approach 1: Deque

**Intuition**

Consider all the nodes numbered first by level and then left to right.  Call this the "number order" of the nodes.

At each insertion step, we want to insert into the node with the lowest number (that still has 0 or 1 children).

By maintaining a `deque` (double ended queue) of these nodes in number order, we can solve the problem.  After inserting a node, that node now has the highest number and no children, so it goes at the end of the deque.  To get the node with the lowest number, we pop from the beginning of the deque.

**Algorithm**

First, perform a breadth-first search to populate the `deque` with nodes that have 0 or 1 children, in number order.

Now when inserting a node, the parent is the first element of `deque`, and we add this new node to our `deque`.


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

* Time Complexity:  The preprocessing is $$O(N)$$, where $$N$$ is the number of nodes in the tree.  Each insertion operation thereafter is $$O(1)$$.

* Space Complexity:  $$O(N_{\text{cur}})$$ space complexity, when the size of the tree during the current insertion operation is $$N_{\text{cur}}$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
