

#### Approach #1: Depth-First Search [Accepted]

**Intuition**

We perform the algorithm explained in the problem description: paint the starting pixels, plus adjacent pixels of the same color, and so on.

**Algorithm**

Say `color` is the color of the starting pixel.  Let's floodfill the starting pixel: we change the color of that pixel to the new color, then check the 4 neighboring pixels to make sure they are valid pixels of the same `color`, and of the valid ones, we floodfill those, and so on.

We can use a function `dfs` to perform a floodfill on a target pixel.


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

* Time Complexity: $$O(N)$$, where $$N$$ is the number of pixels in the image.  We might process every pixel.

* Space Complexity: $$O(N)$$, the size of the implicit call stack when calling `dfs`.

---

Analysis written by: [@awice](https://leetcode.com/awice).
