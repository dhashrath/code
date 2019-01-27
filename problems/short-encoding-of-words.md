

---
#### Approach #1: Store Prefixes [Accepted]

**Intuition**

If a word `X` is a suffix of `Y`, then it does not need to be considered, as the encoding of `Y` in the reference string will also encode `X`.  For example, if `"me"` and `"time"` is in `words`, we can throw out `"me"` without changing the answer.

If a word `Y` does not have any other word `X` (in the list of `words`) that is a suffix of `Y`, then `Y` must be part of the reference string.

Thus, the goal is to remove words from the list such that no word is a suffix of another.  The final answer would be `sum(word.length + 1 for word in words)`.

**Algorithm**

Since a word only has up to 7 suffixes (as `words[i].length <= 7`), let's iterate over all of them.  For each suffix, we'll try to remove it from our `words` list.  For efficiency, we'll make `words` a set.


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

* Time Complexity:  $$O(\sum w_i^2)$$, where $$w_i$$ is the length of `words[i]`.

* Space Complexity: $$O(\sum w_i)$$, the space used in storing suffixes.

---
#### Approach #2: Trie [Accepted]

**Intuition**

As in *Approach #1*, the goal is to remove words that are suffixes of another word in the list.

**Algorithm**

To find whether different words have the same suffix, let's put them backwards into a trie (prefix tree).  For example, if we have `"time"` and `"me"`, we will put `"emit"` and `"em"` into our trie.

After, the leaves of this trie (nodes with no children) represent words that have no suffix, and we will count `sum(word.length + 1 for word in words)`.


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

* Time Complexity:  $$O(\sum w_i)$$, where $$w_i$$ is the length of `words[i]`.

* Space Complexity: $$O(\sum w_i)$$, the space used by the trie.

---

Analysis written by: [@awice](https://leetcode.com/awice).
