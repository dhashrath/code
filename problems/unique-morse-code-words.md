

---
#### Approach #1: Hash Set [Accepted]

**Intuition and Algorithm**

We can transform each `word` into it's Morse Code representation.

After, we put all transformations into a set `seen`, and return the size of the set.


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

* Time Complexity:  $$O(S)$$, where $$S$$ is the sum of the lengths of words in `words`.  We iterate through each character of each word in `words`.

* Space Complexity: $$O(S)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice).
