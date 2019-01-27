

---
#### Approach #1: Minimax with Heuristic [Accepted]

**Intuition**

We can guess that having less words in the word list is generally better.  If the data is random, we can reason this is often the case.

Now let's use the strategy of making the guess that minimizes the maximum possible size of the resulting word list.  If we started with $$N$$ words in our word list, we can iterate through all possibilities for what the secret could be.

**Algorithm**

Store `H[i][j]` as the number of matches of `wordlist[i]` and `wordlist[j]`.  For each guess that hasn't been guessed before, do a minimax as described above, taking the guess that gives us the smallest group that might occur.


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

* Time Complexity:  $$O(N^2 \log N)$$, where $$N$$ is the number of words, and assuming their length is $$O(1)$$.  Each call to `solve` is $$O(N^2)$$, and the number of calls is bounded by $$O(\log N)$$.

* Space Complexity:  $$O(N^2)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice).
