

## Solution
---
#### Approach 1: Reduce to Single Word in B

**Intuition**

If `b` is a subset of `a`, then say `a` is a superset of `b`.  Also, say $$N_{\text{"a"}}(\text{word})$$ is the count of the number of $$\text{"a"}$$'s in the word.

When we check whether a word `wordA` in `A` is a superset of `wordB`, we are individually checking the counts of letters: that for each $$\text{letter}$$, we have $$N_{\text{letter}}(\text{wordA}) \geq N_{\text{letter}}(\text{wordB})$$.

Now, if we check whether a word `wordA` is a superset of all words $$\text{wordB}_i$$, we will check for each letter and each $$i$$, that $$N_{\text{letter}}(\text{wordA}) \geq N_{\text{letter}}(\text{wordB}_i)$$.  This is the same as checking $$N_{\text{letter}}(\text{wordA}) \geq \max\limits_i(N_{\text{letter}}(\text{wordB}_i))$$.

For example, when checking whether `"warrior"` is a superset of words `B = ["wrr", "wa", "or"]`,  we can combine these words in `B` to form a "maximum" word `"arrow"`, that has the maximum count of every letter in each word in `B`.

**Algorithm**

Reduce `B` to a single word `bmax` as described above, then compare the counts of letters between words `a` in `A`, and `bmax`.


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

* Time Complexity:  $$O(\mathcal{A} + \mathcal{B})$$, where $$\mathcal{A}$$ and $$\mathcal{B}$$ is the total amount of information in `A` and `B` respectively.

* Space Complexity:  $$O(A\text{.length} + B\text{.length})$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
