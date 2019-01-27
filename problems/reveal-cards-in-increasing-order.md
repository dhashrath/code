

## Solution
---
#### Approach 1: Simulation

**Intuition and Algorithm**

Simulate the revealing process with a deck set to `[0, 1, 2, ...]`.  If for example this deck is revealed in the order `[0, 2, 4, ...]` then we know we need to put the smallest card in index `0`, the second smallest card in index `2`, the third smallest card in index `4`, etc.


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

* Time Complexity:  $$O(N \log N)$$, where $$N$$ is the length of `deck`.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
