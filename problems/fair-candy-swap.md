

## Solution
---
#### Approach 1: Solve the Equation

**Intuition**

If Alice swaps candy `x`, she expects some specific quantity of candy `y` back.

**Algorithm**

Say Alice and Bob have total candy $$S_A, S_B$$ respectively.

If Alice gives candy $$x$$, and receives candy $$y$$, then Bob receives candy $$x$$ and gives candy $$y$$.  Then, we must have

$$
S_A - x + y = S_B - y + x
$$

for a fair candy swap.  This implies

$$
y = x + \frac{S_B - S_A}{2}
$$

Our strategy is simple.  For every candy $$x$$ that Alice has, if Bob has candy $$y = x + \frac{S_B - S_A}{2}$$, we return $$[x, y]$$.  We use a `Set` structure to check whether Bob has the desired candy $$y$$ in constant time.


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

* Time Complexity:  $$O(A\text{.length} + B\text{.length})$$.

* Space Complexity:  $$O(B\text{.length})$$, the space used by `setB`.  (We can improve this to $$\min(A\text{.length}, B\text{.length})$$ by using an if statement.)
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
