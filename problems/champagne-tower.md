

---
#### Approach #1: Simulation [Accepted]

**Intuition**

Instead of keeping track of how much champagne should end up in a glass, keep track of the total amount of champagne that flows through a glass.  For example, if `poured = 10` cups are poured at the top, then the total flow-through of the top glass is `10`; the total flow-through of each glass in the second row is `4.5`, and so on.

**Algorithm**

In general, if a glass has flow-through `X`, then `Q = (X - 1.0) / 2.0` quantity of champagne will equally flow left and right.  We can simulate the entire pour for 100 rows of glasses.  A glass at `(r, c)` will have excess champagne flow towards `(r+1, c)` and `(r+1, c+1)`.


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

* Time Complexity:  $$O(R^2)$$, where $$R$$ is the number of rows.  As this is fixed, we can consider this complexity to be $$O(1)$$.

* Space Complexity: $$O(R^2)$$, or $$O(1)$$ by the reasoning above.

---

Analysis written by: [@awice](https://leetcode.com/awice).
