

#### Approach #1: Check Position [Accepted]

**Intuition**

If the rectangles do not overlap, then `rec1` must either be higher, lower, to the left, or to the right of `rec2`.

**Algorithm**

The answer for whether they *don't* overlap is `LEFT OR RIGHT OR UP OR DOWN`, where `OR` is the logical OR, and `LEFT` is a boolean that represents whether `rec1` is to the left of `rec2`.  The answer for whether they do overlap is the negation of this.

The condition "`rec1` is to the left of `rec2`" is `rec1[2] <= rec2[0]`, that is the right-most x-coordinate of `rec1` is left of the left-most x-coordinate of `rec2`.  The other cases are similar.


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

* Time and Space Complexity:  $$O(1)$$.

---
#### Approach #2: Check Area [Accepted]

**Intuition**

If the rectangles overlap, they have positive area.  This area must be a rectangle where both dimensions are positive, since the boundaries of the intersection are axis aligned.

Thus, we can reduce the problem to the one-dimensional problem of determining whether two line segments overlap.

**Algorithm**

Say the area of the intersection is `width * height`, where `width` is the intersection of the rectangles projected onto the x-axis, and `height` is the same for the y-axis.  We want both quantities to be positive.

The `width` is positive when `min(rec1[2], rec2[2]) > max(rec1[0], rec2[0])`, that is when the smaller of (the largest x-coordinates) is larger than the larger of (the smallest x-coordinates).  The `height` is similar.


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

* Time and Space Complexity:  $$O(1)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice).
