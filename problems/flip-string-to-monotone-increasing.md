

## Solution
---
#### Approach 1: Prefix Sums

**Intuition**

For say a 5 digit string, the answer is either `'00000'`, `'00001'`, `'00011'`, `'00111'`, `'01111'`, or `'11111'`.  Let's try to calculate the cost of switching to that answer.  The answer has two halves, a left (zero) half, and a right (one) half.

Evidently, it comes down to a question of knowing, for each candidate half: how many ones are in the left half, and how many zeros are in the right half.

We can use prefix sums.  Say `P[i+1] = A[0] + A[1] + ... + A[i]`, where `A[i] = 1` if `S[i] == '1'`, else `A[i] = 0`.  We can calculate `P` in linear time.

Then if we want `x` zeros followed by `N-x` ones, there are `P[x]` ones in the start that must be flipped, plus `(N-x) - (P[N] - P[x])` zeros that must be flipped.  The last calculation comes from the fact that there are `P[N] - P[x]` ones in the later segment of length `N-x`, but we want the number of zeros.

**Algorithm**

For example, with `S = "010110"`:  we have `P = [0, 0, 1, 1, 2, 3, 3]`.  Now say we want to evaluate having `x=3` zeros.

There are `P[3] = 1` ones in the first 3 characters, and `P[6] - P[3] = 2` ones in the later `N-x = 3` characters.

So, there is `(N-x) - (P[N] - P[x]) = 1` zero in the later `N-x` characters.

We take the minimum among all candidate answers to arrive at the final answer.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `S`.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
