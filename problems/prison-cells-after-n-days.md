

## Solution
---
#### Approach 1: Simulation

**Intuition**

We simulate each day of the prison.

Because there are at most 256 possible states for the prison, eventually the states repeat into a cycle rather quickly.  We can keep track of when the states repeat to find the period `t` of this cycle, and skip days in multiples of `t`.

**Algorithm**

Let's do a naive simulation, iterating one day at a time.  For each day, we will decrement `N`, the number of days remaining, and transform the state of the prison forward (`state -> nextDay(state)`).

If we reach a state we have seen before, we know how many days ago it occurred, say `t`.  Then, because of this cycle, we can do `N %= t`.  This ensures that our algorithm only needs $$O(2**{\text{cells.length}})$$ steps.


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

* Time Complexity:  $$O(2^N)$$, where $$N$$ is the number of cells in the prison.

* Space Complexity:  $$O(2^N * N)$$.
<br />
<br />


---
Analysis written by: [@awice](https://leetcode.com/awice).
