

---
#### Approach #1: Two Pointer [Accepted]

**Intuition**

We scan through the string to identify the start and end of each group.  If the size of the group is at least 3, we add it to the answer.

**Algorithm**

Maintain pointers `i, j` with `i <= j`.  The `i` pointer will represent the start of the current group, and we will increment `j` forward until it reaches the end of the group.

We know that we have reached the end of the group when `j` is at the end of the string, or `S[j] != S[j+1]`.  At this point, we have some group `[i, j]`; and after, we will update `i = j+1`, the start of the next group.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `S`.

* Space Complexity: $$O(N)$$, the space used by the answer.

---

Analysis written by: [@awice](https://leetcode.com/awice).
