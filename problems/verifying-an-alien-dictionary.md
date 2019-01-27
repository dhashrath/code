

## Solution
---
#### Approach 1: Check Adjacent Words

**Intuition**

The words are sorted lexicographically if and only if adjacent words are.  This is because order is transitive: `a <= b` and `b <= c` implies `a <= c`.

**Algorithm**

Let's check whether all adjacent words `a` and `b` have `a <= b`.

To check whether `a <= b` for two adjacent words `a` and `b`, we can find their first difference.  For example, `"applying"` and `"apples"` have a first difference of `y` vs `e`.  After, we compare these characters to the index in `order`.

Care must be taken to deal with the blank character effectively.  If for example, we are comparing `"app"` to `"apply"`, this is a first difference of `(null)` vs `"l"`.


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

* Time Complexity:  $$O(\mathcal{C})$$, where $$\mathcal{C}$$ is the total *content* of `words`.

* Space Complexity:  $$O(1)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
