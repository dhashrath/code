

## Solution
---
#### Approach 1: Mathematical

**Intuition and Algorithm**

From the top, the shadow made by the shape will be 1 square for each non-zero value.

From the side, the shadow made by the shape will be the largest value for each row in the grid.

From the front, the shadow made by the shape will be the largest value for each column in the grid.


**Example**

With the example `[[1,2],[3,4]]`:

* The shadow from the top will be 4, since there are four non-zero values in the grid;

* The shadow from the side will be `2 + 4`, since the maximum value of the first row is `2`, and the maximum value of the second row is `4`;

* The shadow from the front will be `3 + 4`, since the maximum value of the first column is `3`, and the maximum value of the second column is `4`.


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

* Time Complexity:  $$O(N^2)$$, where $$N$$ is the length of `grid`.

* Space Complexity:  $$O(1)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
