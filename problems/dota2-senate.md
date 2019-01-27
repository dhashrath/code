

---
#### Approach #1: Simulation [Accepted]

**Intuition**

A senator performing a ban doesn't need to use it on another senator immediately.  We can wait to see when another team's senator will vote, then use that ban retroactively.

**Algorithm**

Put the senators in an integer queue: `1` for `'Radiant'` and `0` for `'Dire'`.

Now process the queue: if there is a floating ban for that senator, exercise it and continue.  Otherwise, add a floating ban against the other team, and enqueue this senator again.


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

* Time Complexity: $$O(N)$$ where $$N$$ is the size of the senate.  Every vote removes one senator from the other team.

* Space Complexity: $$O(N)$$, the space used by our queue.

---

Analysis written by: [@awice](https://leetcode.com/awice).
