

---
#### Approach #1: Run Length Encoding [Accepted]

**Intuition**

For some word, write the head character of every group, and the count of that group.  For example, for `"abbcccddddaaaaa"`, we'll write the "key" of `"abcda"`, and the "count" `[1,2,3,4,5]`.

Let's see if a `word` is stretchy.  Evidently, it needs to have the same key as `S`.

Now, let's say we have individual counts `c1 = S.count[i]` and `c2 = word.count[i]`.

* If `c1 < c2`, then we can't make the `i`th group of `word` equal to the `i`th word of `S` by adding characters.

* If `c1 >= 3`, then we can add letters to the `i`th group of `word` to match the `i`th group of `S`, as the latter is *extended*.

* Else, if `c1 < 3`, then we must have `c2 == c1` for the `i`th groups of `word` and `S` to match.


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

* Time Complexity:  $$O(QK)$$, where $$Q$$ is the length of `words` (at least 1), and $$K$$ is the maximum length of a word.

* Space Complexity: $$O(K)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice).
