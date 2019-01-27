

## Solution
---
#### Approach 1: Square by Square

**Intuition**

Let's try to count the surface area contributed by `v = grid[i][j]`.

When `v > 0`, the top and bottom surface contributes an area of 2.

Then, for each side (west side, north side, east side, south side) of the column at `grid[i][j]`, the neighboring cell with value `nv` means our square contributes `max(v - nv, 0)`.

For example, for `grid = [[1, 5]]`, the contribution at `grid[0][1]` is 2 + 5 + 5 + 5 + 4.  The 2 comes from the top and bottom side, the 5 comes from the north, east, and south side; and the 4 comes from the west side, of which 1 unit is covered by the adjacent column.

**Algorithm**

For each `v = grid[r][c] > 0`, count `ans += 2`, plus `ans += max(v - nv, 0)` for each neighboring value `nv` adjacent to `grid[r][c]`.


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

* Time Complexity:  $$O(N^2)$$, where $$N$$ is the number of rows (and columns) in the `grid`.

* Space Complexity:  $$O(1)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
