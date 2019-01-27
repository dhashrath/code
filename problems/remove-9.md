

---
#### Approach #1: Mathematical

**Intuition**

Let's write the first numbers and try to notice a pattern.  Those numbers are:

```python
1, 2, 3, 4, 5, 6, 7, 8,
10, 11, 12, 13, 14, 15, 16, 17, 18,
20, 21, 22, 23, 24, 25, 26, 27, 28,
...
80, 81, 82, 83, 84, 85, 86, 87, 88,
100, 101, 102, ...
```

These numbers look exactly like all base-9 numbers!

Indeed, every base-9 number is a number in this sequence, and every number in this sequence is a base-9 number.  Both this sequence and the sequence of all base-9 numbers are in increasing order.  The answer is therefore just the n-th base-9 number.


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

* Time Complexity: $$O(1)$$, since $$N$$ has at most 9 digits.

* Space Complexity: $$O(1)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice).