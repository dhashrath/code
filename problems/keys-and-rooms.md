

---
#### Approach #1: Depth-First Search [Accepted]

**Intuition and Algorithm**

When visiting a room for the first time, look at all the keys in that room.  For any key that hasn't been used yet, add it to the todo list (`stack`) for it to be used.

See the comments of the code for more details.


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

* Time Complexity:  $$O(N + E)$$, where $$N$$ is the number of rooms, and $$E$$ is the total number of keys.

* Space Complexity:  $$O(N)$$ in additional space complexity, to store `stack` and `seen`.

---

Analysis written by: [@awice](https://leetcode.com/awice).
