

---
#### Approach #1: Brute Force [Accepted]

**Intuition**

The first two elements of the array uniquely determine the rest of the sequence.

**Algorithm**

For each of the first two elements, assuming they have no leading zero, let's iterate through the rest of the string.  At each stage, we expect a number less than or equal to `2^31 - 1` that starts with the sum of the two previous numbers.


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

* Time Complexity:  $$O(N^2)$$, where $$N$$ is the length of `S`, and with the requirement that the values of the answer are $$O(1)$$ in length.

* Space Complexity:  $$O(N)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice).
