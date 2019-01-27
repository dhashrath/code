

## Solution
---
#### Approach 1: Recursion

**Intuition and Algorithm**

Let $$\text{FBT}(N)$$ be the list of all possible full binary trees with $$N$$ nodes.

Every full binary tree $$T$$ with 3 or more nodes, has 2 children at its root.  Each of those children `left` and `right` are themselves full binary trees.

Thus, for $$N \geq 3$$, we can formulate the recursion: $$\text{FBT}(N) =$$ [All trees with left child from $$\text{FBT}(x)$$ and right child from $$\text{FBT}(N-1-x)$$, for all $$x$$].

Also, by a simple counting argument, there are no full binary trees with a positive, even number of nodes.

Finally, we should cache previous results of the function $$\text{FBT}$$ so that we don't have to recalculate them in our recursion.


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

* Time Complexity:  $$O(2^N)$$.  For odd $$N$$, let $$N = 2k + 1$$.  Then, $$\Big| \text{FBT}(N) \Big| = C_k$$, the $$k$$-th catalan number; and $$\sum\limits_{k < \frac{N}{2}} C_k$$ (the complexity involved in computing intermediate results required) is bounded by $$O(2^N)$$.  However, the proof is beyond the scope of this article.

* Space Complexity:  $$O(2^N)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
