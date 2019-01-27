

---
#### Approach #1: Brute Force [Accepted]

**Intuition**

We will repeatedly try to form a group (of size W) starting with the lowest card.  This works because the lowest card still in our hand must be the bottom end of a size `W` straight.

**Algorithm**

Let's keep a count `{card: number of copies of card}` as a `TreeMap` (or `dict`).

Then, repeatedly we will do the following steps: find the lowest value card that has 1 or more copies (say with value `x`), and try to remove `x, x+1, x+2, ..., x+W-1` from our count.  If we can't, then the task is impossible.


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

* Time Complexity:  $$O(N * (N/W))$$, where $$N$$ is the length of `hand`.  The $$(N / W)$$ factor comes from `min(count)`.  In Java, the $$(N / W)$$ factor becomes $$\log N$$ due to the complexity of `TreeMap`.

* Space Complexity:  $$O(N)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice).
