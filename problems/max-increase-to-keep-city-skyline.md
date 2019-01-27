

---
#### Approach #1: Row and Column Maximums [Accepted]

**Intuition and Algorithm**

The skyline looking from the top is `col_maxes = [max(column_0), max(column_1), ...]`.  Similarly, the skyline from the left is `row_maxes [max(row_0), max(row_1), ...]`

In particular, each building `grid[r][c]` could become height `min(max(row_r), max(col_c))`, and this is the largest such height.  If it were larger, say `grid[r][c] > max(row_r)`, then the part of the skyline `row_maxes = [..., max(row_r), ...]` would change.

These increases are also independent (none of them change the skyline), so we can perform them independently.


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

* Time Complexity:  $$O(N^2)$$, where $$N$$ is the number of rows (and columns) of the grid.  We iterate through every cell of the grid.

* Space Complexity: $$O(N)$$, the space used by `row_maxes` and `col_maxes`.

---

Analysis written by: [@awice](https://leetcode.com/awice).
