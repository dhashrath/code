

---
#### Approach #1: Direct [Accepted]

**Intuition and Algorithm**

We can do this in place.  In each row, the `i`th value from the left is equal to the inverse of the `i`th value from the right.

We use `(C+1) / 2` (with floor division) to iterate over all indexes `i` in the first half of the row, including the center.


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

* Time Complexity:  $$O(N)$$, where `N` is the total number of elements in `A`.

* Space Complexity: $$O(1)$$ in *additional* space complexity.

---

Analysis written by: [@awice](https://leetcode.com/awice).
