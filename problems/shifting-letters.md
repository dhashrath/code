

---
#### Approach #1: Prefix Sum [Accepted]

**Intuition**

Let's ask how many times the `i`th character is shifted.

**Algorithm**

The `i`th character is shifted `shifts[i] + shifts[i+1] + ... + shifts[shifts.length - 1]` times.  That's because only operations at the `i`th operation and after, affect the `i`th character.

Let `X` be the number of times the current `i`th character is shifted.  Then the next character `i+1` is shifted `X - shifts[i]` times.

For example, if `S.length = 4` and `S[0]` is shifted `X = shifts[0] + shifts[1] + shifts[2] + shifts[3]` times, then `S[1]` is shifted `shifts[1] + shifts[2] + shifts[3]` times, `S[2]` is shifted `shifts[2] + shifts[3]` times, and so on.

In general, we need to do `X -= shifts[i]` to maintain the correct value of `X` as we increment `i`.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `S` (and `shifts`).

* Space Complexity:  $$O(N)$$, the space needed to output the answer.

---

Analysis written by: [@awice](https://leetcode.com/awice).
