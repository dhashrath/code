

---
#### Approach #1: Iterate Through Grid

**Intuition and Algorithm**

For each cell in the grid, look at the immediate neighbors - up to 9 of them, including the original cell.

Then, we will add the sum of the neighbors into `ans[r][c]` while recording `count`, the number of such neighbors.  The final answer is the sum divided by the count.


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

* Time Complexity: $$O(N)$$, where $$N$$ is the number of pixels in our image.  We iterate over every pixel.

* Space Complexity: $$O(N)$$, the size of our answer.

---

Analysis written by: [@awice](https://leetcode.com/awice).
