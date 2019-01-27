

## Solution
---
#### Approach 1: Mathematical

**Intuition and Algorithm**

Let `A` be the original array, and `B` be the array after all our modifications.  Towards trying to minimize `max(B) - min(B)`, let's try to minimize `max(B)` and maximize `min(B)` separately.

The smallest possible value of `max(B)` is `max(A) - K`, as the value `max(A)` cannot go lower.  Similarly, the largest possible value of `min(B)` is `min(A) + K`.  So the quantity `max(B) - min(B)` is at least `ans = (max(A) - K) - (min(A) + K)`.

We can attain this value (if `ans >= 0`), by the following modifications:

* If $$A[i] \leq \min(A) + K$$, then $$B[i] = \min(A) + K$$
* Else, if $$A[i] \geq \max(A) - K$$, then $$B[i] = \max(A) - K$$
* Else, $$B[i] = A[i]$$.

If `ans < 0`, the best answer we could have is `ans = 0`, also using the same modification.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `A`.

* Space Complexity:  $$O(1)$$.
<br />
<br />


---

Analysis written by: [@awice](https://leetcode.com/awice).
