

#### Approach #1: Brute Force [Accepted]

**Intuition and Algorithm**

For each number in the given range, we will directly test if that number is self-dividing.

By definition, we want to test each whether each digit is non-zero and divides the number.  For example, with `128`, we want to test `d != 0 && 128 % d == 0` for `d = 1, 2, 8`.  To do that, we need to iterate over each digit of the number.

A straightforward approach to that problem would be to convert the number into a character array (string in Python), and then convert back to integer to perform the modulo operation when checking `n % d == 0`.

We could also continually divide the number by 10 and peek at the last digit.  That is shown as a variation in a comment.


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

* Time Complexity: $$O(D)$$, where $$D$$ is the number of integers in the range $$[L, R]$$, and assuming $$\log(R)$$ is bounded.  (In general, the complexity would be $$O(D\log R)$$.)

* Space Complexity: $$O(D)$$, the length of the answer.

---

Analysis written by: [@awice](https://leetcode.com/awice).
