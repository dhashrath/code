

## Solution
---
#### Approach 1: Brute Force

**Intuition**

Let's try to write some number in the answer digit by digit.

For each digit except the first, there are at most 2 choices for that digit.  This means that there are at most $$9 * 2^8$$ possible 9 digit numbers, for example.  This is small enough to brute force.

**Algorithm**

An $$N$$ digit number is just an $$N-1$$ digit number with a final digit added.  If the $$N-1$$ digit number ends in a digit $$d$$, then the $$N$$ digit number will end in $$d-K$$ or $$d+K$$ (provided these are digits in the range $$[0,9]$$).  We store these numbers in a `Set` structure to avoid duplicates.

Also, we should be careful about leading zeroes -- only 1 digit numbers will start with `0`.


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

* Time Complexity:  $$O(2^N)$$.

* Space Complexity:  $$O(2^N)$$.
<br />
<br />


---
Analysis written by: [@awice](https://leetcode.com/awice).
