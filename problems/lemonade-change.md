

## Solution
---
#### Approach 1: Simulation

**Intuition and Algorithm**

Let's try to simulate giving change to each customer buying lemonade.  Initially, we start with no `five` dollar bills, and no `ten` dollar bills.

* If a customer brings a $5 bill, then we take it.

* If a customer brings a $10 bill, we must return a five dollar bill.  If we don't have a five dollar bill, the answer is `False`, since we can't make correct change.

* If a customer brings a $20 bill, we must return $15.

    * If we have a $10 and a $5, then we always prefer giving change in that, because it is strictly worse for making change than three $5 bills.

    * Otherwise, if we have three $5 bills, then we'll give that.

    * Otherwise, we won't be able to give $15 in change, and the answer is `False`.



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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `bills`.

* Space Complexity:  $$O(1)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
