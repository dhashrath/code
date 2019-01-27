

---
#### Approach #1: Counting [Accepted]

**Intuition**

Instead of processing all `20000` people, we can process pairs of `(age, count)` representing how many people are that age.  Since there are only 120 possible ages, this is a much faster loop.

**Algorithm**

For each pair `(ageA, countA)`, `(ageB, countB)`, if the conditions are satisfied with respect to age, then `countA * countB` pairs of people made friend requests.

If `ageA == ageB`, then we overcounted: we should have `countA * (countA - 1)` pairs of people making friend requests instead, as you cannot friend request yourself.


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

* Time Complexity:  $$O(\mathcal{A}^2 + N)$$, where $$N$$ is the number of people, and $$\mathcal{A}$$ is the number of ages.

* Space Complexity: $$O(\mathcal{A})$$, the space used to store `count`.

---

Analysis written by: [@awice](https://leetcode.com/awice).
