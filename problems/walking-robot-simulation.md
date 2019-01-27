

## Solution
---
#### Approach 1: Simulation

**Intuition**

We simulate the path of the robot step by step.  Since there are at most 90000 steps, this is efficient enough to pass the given input limits.

**Algorithm**

We store the robot's position and direction.  If we get a turning command, we update the direction; otherwise we walk the specified number of steps in the given direction.

Care must be made to use a `Set` data structure for the obstacles, so that we can check efficiently if our next step is obstructed.  If we don't, our check `is point in obstacles` could be ~10,000 times slower.

In some languages, we need to encode the coordinates of each obstacle as a `long` integer so that it is a hashable key that we can put into a `Set` data structure.  Alternatively, we could also encode the coordinates as a `string`.


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

* Time Complexity:  $$O(N + K)$$, where $$N, K$$ are the lengths of `commands` and `obstacles` respectively.

* Space Complexity:  $$O(K)$$, the space used in storing the `obstacleSet`.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
