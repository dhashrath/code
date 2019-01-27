

---
#### Approach #1: Prefix Hash [Accepted]

**Intuition**

For each word in the sentence, we'll look at successive prefixes and see if we saw them before.

**Algorithm**

Store all the `roots` in a *Set* structure.  Then for each word, look at successive prefixes of that word.  If you find a prefix that is a root, replace the word with that prefix.  Otherwise, the prefix will just be the word itself, and we should add that to the final sentence answer.


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

* Time Complexity: $$O(\sum w_i^2)$$ where $$w_i$$ is the length of the $$i$$-th word.  We might check every prefix, the $$i$$-th of which is $$O(w_i^2)$$ work.

* Space Complexity: $$O(N)$$ where $$N$$ is the length of our sentence; the space used by `rootset`.

---
#### Approach #2: Trie [Accepted]

**Intuition and Algorithm**

Put all the roots in a trie (prefix tree).  Then for any query word, we can find the smallest root that was a prefix in linear time.


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

* Time Complexity: $$O(N)$$ where $$N$$ is the length of the `sentence`.  Every query of a word is in linear time.

* Space Complexity: $$O(N)$$, the size of our trie.

---

Analysis written by: [@awice](https://leetcode.com/awice).
