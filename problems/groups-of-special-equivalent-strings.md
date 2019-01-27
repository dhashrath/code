

## Solution
---
#### Approach 1: Counting

**Intuition and Algorithm**

Let's try to characterize a special-equivalent string $$S$$, by finding a function $$\mathcal{C}$$ so that $$S \equiv T \iff \mathcal{C}(S) = \mathcal{C}(T)$$.

Through swapping, we can permute the even indexed letters, and the odd indexed letters.  What characterizes these permutations is the count of the letters: all such permutations have the same count, and different counts have different permutations.

Thus, the function $$\mathcal{C}(S) =$$ (the count of the even indexed letters in S, followed by the count of the odd indexed letters in S) successfully characterizes the equivalence relation.

Afterwards, we count the number of unique $$\mathcal{C}(S)$$ for $$S \in A$$.


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

* Time Complexity:  $$O(\sum\limits_{i} (A_i)\text{.length})$$

* Space Complexity:  $$O(N)$$, where $$N$$ is the length of `A`.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
