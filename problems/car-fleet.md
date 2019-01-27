

---
#### Approach 1: Stack

**Intuition**

Call the "lead fleet" the fleet furthest in position.

If the car `S` (Second) behind the lead car `F` (First) would arrive earlier, then `S` forms a fleet with the lead car `F`.  Otherwise, fleet `F` is final as no car can catch up to it - cars behind `S` would form fleets with `S`, never `F`.

**Algorithm**

A car is a `(position, speed)` which implies some arrival time `(target - position) / speed`.  Sort the cars by position.

Now apply the above reasoning - if the lead fleet drives away, then count it and continue.  Otherwise, merge the fleets and continue.


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

* Time Complexity:  $$O(N \log N)$$, where $$N$$ is the number of cars.  The complexity is dominated by the sorting operation.

* Space Complexity:  $$O(N)$$, the space used to store information about the cars.

---

Analysis written by: [@awice](https://leetcode.com/awice).
