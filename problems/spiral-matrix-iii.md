

## Solution
---
#### Approach 1: Walk in a Spiral

**Intuition**

We can walk in a spiral shape from the starting square, ignoring whether we stay in the grid or not.  Eventually, we must have reached every square in the grid.

**Algorithm**

Examining the lengths of our walk in each direction, we find the following pattern: `1, 1, 2, 2, 3, 3, 4, 4, ...`  That is, we walk 1 unit east, then 1 unit south, then 2 units west, then 2 units north, then 3 units east, etc.  Because our walk is self-similar, this pattern repeats in the way we expect.

After, the algorithm is straightforward: perform the walk and record positions of the grid in the order we visit them.  Please read the inline comments for more details.


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

* Time Complexity:  $$O((\max(R, C))^2)$$.  Potentially, our walk needs to spiral until we move $$R$$ in one direction, and $$C$$ in another direction, so as to reach every cell of the grid.

* Space Complexity:  $$O(R * C)$$, the space used by the answer.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
