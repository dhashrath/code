

---
#### Approach #1: String [Accepted]

**Intuition**

We apply the steps given in the problem in a straightforward manner.  The difficulty lies in the implementation.

**Algorithm**

For each `word` in the sentence split, if it is a vowel we consider the word, otherwise we consider the rotation of the word (either `word[1:] + word[:1]` in Python, otherwise `word.substring(1) + word.substring(0, 1)` in Java).

Afterwards, we add `"ma"`, the desired number of `"a"`'s, and a space character.


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

* Time Complexity:  $$O(N^2)$$, where $$N$$ is the length of `S`.  This represents the complexity of rotating the word and adding extra `"a"` characters.

* Space Complexity: $$O(N^2)$$, the space added to the answer by adding extra `"a"` characters.

---

Analysis written by: [@awice](https://leetcode.com/awice).
