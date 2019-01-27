

---
#### Approach #1: Direct [Accepted]

**Intuition and Algorithm**

We showcase two different approaches.  In both approaches, we build some answer string `ans`, that starts as `S`.  Our main motivation in these approaches is to be able to identify and handle when a given replacement operation does nothing.

In *Java*, the idea is to build an array `match` that tells us `match[ix] = j` whenever `S[ix]` is the head of a successful replacement operation `j`: that is, whenever `S[ix:].startswith(sources[j])`.

After, we build the answer using this match array.  For each index `ix` in `S`, we can use `match` to check whether `S[ix]` is being replaced or not.  We repeatedly either write the next character `S[ix]`, or groups of characters `targets[match[ix]]`, depending on the value of `match[ix]`.

In *Python*, we sort our replacement jobs `(i, x, y)` in reverse order.  If `S[i:].startswith(x)`, then we can replace that section `S[i:i+len(x)]` with the target `y`.  We used a reverse order so that edits to `S` do not interfere with the rest of the queries.


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

* Time Complexity: $$O(NQ)$$, where $$N$$ is the length of `S`, and we have $$Q$$ replacement operations.  (Our complexity could be faster with a more accurate implementation, but it isn't necessary.)

* Space Complexity: $$O(N)$$, if we consider `targets[i].length <= 100` as a constant bound.

---

Analysis written by: [@awice](https://leetcode.com/awice).
