

## Solution
---
#### Approach 1: Queue

**Intuition**

We only care about the most recent calls in the last 3000 ms, so let's use a data structure that keeps only those.

**Algorithm**

Keep a queue of the most recent calls in increasing order of `t`.  When we see a new call with time `t`, remove all calls that occurred before `t - 3000`.


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

* Time Complexity:  $$O(Q)$$, where $$Q$$ is the number of queries made.

* Space Complexity:  $$O(W)$$, where $$W = 3000$$ is the size of the window we should scan for recent calls.  In this problem, the complexity can be considered $$O(1)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
