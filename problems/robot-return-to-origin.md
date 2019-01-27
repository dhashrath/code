

---
#### Approach #1: Simulation [Accepted]

**Intuition**

We can simulate the position of the robot after each command.

**Algorithm**

Initially, the robot is at `(x, y) = (0, 0)`.  If the move is `'U'`, the robot goes to `(x, y-1)`; if the move is `'R'`, the robot goes to `(x, y) = (x+1, y)`, and so on.


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

* Time Complexity: $$O(N)$$, where $$N$$ is the length of `moves`.  We iterate through the string.

* Space Complexity: $$O(1)$$.  In Java, our character array is $$O(N)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice).
