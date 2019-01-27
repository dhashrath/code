

---
#### Approach #1: Min Array [Accepted]

**Intuition**

For each index `S[i]`, let's try to find the distance to the next character `C` going left, and going right.  The answer is the minimum of these two values.

**Algorithm**

When going left to right, we'll remember the index `prev` of the last character `C` we've seen.  Then the answer is `i - prev`.

When going right to left, we'll remember the index `prev` of the last character `C` we've seen.  Then the answer is `prev - i`.

We take the minimum of these two answers to create our final answer.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `S`.  We scan through the string twice.

* Space Complexity: $$O(N)$$, the size of `ans`.

---

Analysis written by: [@awice](https://leetcode.com/awice).
