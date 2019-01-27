

## Solution
---
#### Approach 1: Store Exhausted Position and Quantity

**Intuition**

We can store an index `i` and quantity `q` which represents that `q` elements of `A[i]` (repeated `A[i+1]` times) are exhausted.

For example, if we have `A = [1,2,3,4]` (mapping to the sequence `[2,4,4,4]`) then `i = 0, q = 0` represents that nothing is exhausted; `i = 0, q = 1` represents that `[2]` is exhausted, `i = 2, q = 1` will represent that we have currently exhausted `[2, 4]`, and so on.

**Algorithm**

Say we want to exhaust `n` more elements.  There are currently `D = A[i] - q` elements left to exhaust (of value `A[i+1]`).

If `n > D`, then we should exhaust all of them and continue: `n -= D; i += 2; q = 0`.

Otherwise, we should exhaust some of them and return the current element's value: `q += D; return A[i+1]`.


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

* Time Complexity:  $$O(N + Q)$$, where $$N$$ is the length of `A`, and $$Q$$ is the number of calls to `RLEIterator.next`.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
