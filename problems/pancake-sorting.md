

## Solution
---
#### Approach 1: Sort Largest to Smallest

**Intuition**

We can place the largest element (in location `i`, 1-indexed) by flipping `i` to move the element to the first position, then `A.length` to move it to the last position.  Afterwards, the largest element is in the correct position, so we no longer need to pancake-flip by `A.length` or more.

We can repeat this process until the array is sorted.  Each move will use 2 flips per element.

**Algorithm**

First, sort the locations from largest value of A to smallest value of A.

For each element (from largest to smallest) with location `i`, we will first simulate where this element actually is, based on the pancake flips we have done.  For a pancake flip `f`, if `i <= f`, then the element has moved from location `i` to `f+1 - i`.

After, we flip by `i` then `N--` to put this element in the correct position.


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

* Time Complexity:  $$O(N^2)$$, where $$N$$ is the length of `A`.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---
Analysis written by: [@awice](https://leetcode.com/awice).
