

## Solution
---
#### Approach 1: Stack of Stacks

**Intuition**

Evidently, we care about the frequency of an element.  Let `freq` be a `Map` from $$x$$ to the number of occurrences of $$x$$.

Also, we (probably) care about `maxfreq`, the current maximum frequency of any element in the stack.  This is clear because we must pop the element with the maximum frequency.

The main question then becomes: among elements with the same (maximum) frequency, how do we know which element is most recent?  We can use a stack to query this information: the top of the stack is the most recent.

To this end, let `group` be a map from frequency to a stack of elements with that frequency.  We now have all the required components to implement `FreqStack`.

**Algorithm**

Actually, as an implementation level detail, if `x` has frequency `f`, then we'll have `x` in all `group[i] (i <= f)`, not just the top.  This is because each `group[i]` will store information related to the `i`th copy of `x`.

Afterwards, our goal is just to maintain `freq`, `group`, and `maxfreq` as described above.


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

* Time Complexity:  $$O(1)$$ for both `push` and `pop` operations.

* Space Complexity:  $$O(N)$$, where `N` is the number of elements in the `FreqStack`.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
