

## Solution
---
#### Approach 1: Depth-First Search

**Intuition**

As we do a pre-order traversal, we will flip nodes on the fly to try to match our voyage with the given one.

If we are expecting the next integer in our voyage to be `voyage[i]`, then there is only at most one choice for path to take, as all nodes have different values.

**Algorithm**

Do a depth first search.  If at any node, the node's value doesn't match the voyage, the answer is `[-1]`.

Otherwise, we know when to flip: the next number we are expecting in the voyage `voyage[i]` is different from the next child.


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
