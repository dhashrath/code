

## Solution
---
#### Approach 1: Greedy

**Intuition**

If the smallest card `a` in `A` beats the smallest card `b` in `B`, we should pair them.  Otherwise, `a` is useless for our score, as it can't beat any cards.

Why should we pair `a` and `b` if `a > b`?  Because every card in `A` is larger than `b`, any card we place in front of `b` will score a point.  We might as well use the weakest card to pair with `b` as it makes the rest of the cards in `A` strictly larger, and thus have more potential to score points.

**Algorithm**

We can use the above intuition to create a greedy approach.  The current smallest card to beat in `B` will always be `b = sortedB[j]`.  For each card `a` in `sortedA`, we will either have `a` beat that card `b` (put `a` into `assigned[b]`), or throw `a` out (put `a` into `remaining`).

Afterwards, we can use our annotations `assigned` and `remaining` to reconstruct the answer.  Please see the comments for more details.



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

* Time Complexity:  $$O(N \log N)$$, where $$N$$ is the length of `A` and `B`.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
