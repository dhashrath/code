

#### Approach #1: Stack [Accepted]

**Intuition**

A row of asteroids is stable if no further collisions will occur.  After adding a new asteroid to the right, some more collisions may happen before it becomes stable again, and all of those collisions (if they happen) must occur right to left.  This is the perfect situation for using a *stack*.

**Algorithm**

Say we have our answer as a stack with rightmost asteroid `top`, and a `new` asteroid comes in.  If `new` is moving right (`new > 0`), or if `top` is moving left (`top < 0`), no collision occurs.

Otherwise, if `abs(new) < abs(top)`, then the `new` asteroid will blow up; if `abs(new) == abs(top)` then both asteroids will blow up; and if `abs(new) > abs(top)`, then the `top` asteroid will blow up (and possibly more asteroids will, so we should continue checking.)


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

* Time Complexity: $$O(N)$$, where $$N$$ is the number of asteroids.  Our stack pushes and pops each asteroid at most once.

* Space Complexity: $$O(N)$$, the size of `ans`.

---

Analysis written by: [@awice](https://leetcode.com/awice).
