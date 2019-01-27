

---
#### Approach #1: Hash Set [Accepted]

**Intuition**

If a card has the same value `x` on the front and back, it is impossible to win with `x`.  Otherwise, it has two different values, and if we win with `x`, we can put `x` face down on the rest of the cards.

**Algorithm**

Remember all values `same` that occur twice on a single card.  Then for every value `x` on any card that isn't in `same`, `x` is a candidate answer.  If we have no candidate answers, the final answer is zero.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `fronts` (and `backs`).  We scan through the arrays.

* Space Complexity: $$O(N)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice).
