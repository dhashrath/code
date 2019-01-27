

---
#### Approach #1: Brute Force [Accepted]

**Intuition and Algorithm**

Let's try to find the smallest left-most chunk.  If the first `k` elements are `[0, 1, ..., k-1]`, then it can be broken into a chunk, and we have a smaller instance of the same problem.

We can check whether `k+1` elements chosen from `[0, 1, ..., n-1]` are `[0, 1, ..., k]` by checking whether the maximum of that choice is `k`.


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

* Time Complexity: $$O(N)$$, where $$N$$ is the length of `arr`

* Space Complexity: $$O(1)$$.

---

For more approaches, please visit the article for the companion problem [Max Chunks To Make Sorted II](https://leetcode.com/articles/max-chunks-to-make-sorted-ii/).

---

Analysis written by: [@awice](https://leetcode.com/awice).
