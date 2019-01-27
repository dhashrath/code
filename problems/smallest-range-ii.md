

## Solution
---
#### Approach 1: Linear Scan

**Intuition**

As in *Smallest Range I*, smaller `A[i]` will choose to increase their value ("go up"), and bigger `A[i]` will decrease their value ("go down").

**Algorithm**

We can formalize the above concept: if `A[i] < A[j]`, we don't need to consider when `A[i]` goes down while `A[j]` goes up.  This is because the interval `(A[i] + K, A[j] - K)` is a subset of `(A[i] - K, A[j] + K)` (here, `(a, b)` for `a > b` denotes `(b, a)` instead.)

That means that it is never worse to choose `(up, down)` instead of `(down, up)`.  We can prove this claim that one interval is a subset of another, by showing both `A[i] + K` and `A[j] - K` are between `A[i] - K` and `A[j] + K`.

For sorted `A`, say `A[i]` is the largest `i` that goes up.  Then `A[0] + K, A[i] + K, A[i+1] - K, A[A.length - 1] - K` are the only relevant values for calculating the answer: every other value is between one of these extremal values.


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

* Time Complexity:  $$O(N \log N)$$, where $$N$$ is the length of the `A`.

* Space Complexity:  $$O(1)$$, plus the space used by the builtin sorting algorithm.
<br />
<br />


---

Analysis written by: [@awice](https://leetcode.com/awice).
