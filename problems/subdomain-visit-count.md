

---
#### Approach #1: Hash Map [Accepted]

**Intuition and Algorithm**

The algorithm is straightforward: we just do what the problem statement tells us to do.

For an address like `a.b.c`, we will count `a.b.c`, `b.c`, and `c`.  For an address like `x.y`, we will count `x.y` and `y`.

To count these strings, we will use a hash map.  To split the strings into the required pieces, we will use library `split` functions.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `cpdomains`, and assuming the length of `cpdomains[i]` is fixed.

* Space Complexity: $$O(N)$$, the space used in our count.

---

Analysis written by: [@awice](https://leetcode.com/awice).
