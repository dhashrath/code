

---
#### Approach #1: Insert Each Character [Accepted]

**Intuition**

We can write out each character in the string `S` one by one.

As we write characters, we can update `(lines, width)` that keeps track of how many lines we have used, and what is the length of the used space in the last line.

**Algorithm**

If the space `w` of the next character in `S` fits our current line, we will add it.  Otherwise, we will start a new line, and use `w` space to put that character on the next line.


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

* Time Complexity:  $$O(S\text{.length})$$, as we iterate through `S`.

* Space Complexity: $$O(1)$$ additional space, as we only use `lines` and `width`.  (In Java, our `toCharArray` method makes this $$O(S\text{.length})$$, but we could use `.charAt` instead).

---

Analysis written by: [@awice](https://leetcode.com/awice).
