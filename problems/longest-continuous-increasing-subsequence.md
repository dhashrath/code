


#### Approach #1: Sliding Window [Accepted]

**Intuition and Algorithm**

Every (continuous) increasing subsequence is disjoint, and the boundary of each such subsequence occurs whenever `nums[i-1] >= nums[i]`.  When it does, it marks the start of a new increasing subsequence at `nums[i]`, and we store such `i` in the variable `anchor`.

For example, if `nums = [7, 8, 9, 1, 2, 3]`, then `anchor` starts at `0` (`nums[anchor] = 7`) and gets set again to `anchor = 3` (`nums[anchor] = 1`).  Regardless of the value of `anchor`, we record a candidate answer of `i - anchor + 1`, the length of the subarray `nums[anchor], nums[anchor+1], ..., nums[i]`; and our answer gets updated appropriately.


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

* Time Complexity: $$O(N)$$, where $$N$$ is the length of `nums`.  We perform one loop through `nums`.

* Space Complexity: $$O(1)$$, the space used by `anchor` and `ans`.

---

Analysis written by: [@awice](https://leetcode.com/awice).
