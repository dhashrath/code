

---
#### Approach #1: Direct [Accepted]

**Intuition and Algorithm**

We will reverse each block of `2k` characters directly.

Each block starts at a multiple of `2k`: for example, `0, 2k, 4k, 6k, ...`.  One thing to be careful about is we may not reverse each block if there aren't enough characters.

To reverse a block of characters from `i` to `j`, we can swap characters in positions `i++` and `j--`.


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

* Time Complexity: $$O(N)$$, where $$N$$ is the size of `s`.  We build a helper array, plus reverse about half the characters in `s`.

* Space Complexity: $$O(N)$$, the size of `a`.

---

Analysis written by: [@awice](https://leetcode.com/awice).
