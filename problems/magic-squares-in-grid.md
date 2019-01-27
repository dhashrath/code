

---
#### Approach #1: Brute Force [Accepted]

**Intuition and Algorithm**

Let's check every 3x3 grid individually.  For each grid, all numbers must be unique and between 1 and 9; plus every row, column, and diagonal must have the same sum.

**Extra Credit**

We could also include an `if grid[r+1][c+1] != 5: continue` check into our code, helping us skip over our `for r... for c...` for loops faster.  This is based on the following observations:

* The sum of the grid must be 45, as it is the sum of the distinct values from 1 to 9.
* Each horizontal and vertical line must add up to 15, as the sum of 3 of these lines equals the sum of the whole grid.
* The diagonal lines must also sum to 15, by definition of the problem statement.
* Adding the 12 values from the four lines that cross the center, these 4 lines add up to 60; but they also add up to the entire grid (45), plus 3 times the middle value.  This implies the middle value is 5.


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

* Time Complexity:  $$O(R*C)$$, where $$R, C$$ are the number of rows and columns in the given `grid`.

* Space Complexity:  $$O(1)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice).
