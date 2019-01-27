

## Solution
---
#### Approach 1: Brute Force

**Intuition**

Try all possible times, and remember the largest one.

**Algorithm (Java)**

Iterate over all permutations `(i, j, k, l)` of `(0, 1, 2, 3)`.  For each permutation, we can try the time `A[i]A[j] : A[k]A[l]`.

This is a valid time if and only if the number of hours `10*A[i] + A[j]` is less than `24`; and the number of minutes `10*A[k] + A[l]` is less than `60`.

We will output the largest valid time.

**Algorithm (Python)**

For each possible ordering of the 4 digits, if it's a legal time and the time is greater than the one we have stored, update the answer.


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

* Time Complexity:  $$O(1)$$.

* Space Complexity:  $$O(1)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).  Java implementation inspired by [@rock](https://leetcode.com/problems/largest-time-for-given-digits/discuss/200693/Java-11-liner-O(64)-w-comment-6-ms.).
