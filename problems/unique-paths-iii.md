

## Solution
---
#### Approach 1: Backtracking DFS

**Intuition and Algorithm**

Let's try walking to each `0`, leaving an obstacle behind from where we walked.  After, we can remove the obstacle.

Given the input limits, this can work because bad paths tend to get stuck quickly and run out of free squares.


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

* Time Complexity:  $$O(4^{R*C})$$, where $$R, C$$ are the number of rows and columns in the grid.  (We can find tighter bounds, but such a bound is beyond the scope of this article.)

* Space Complexity:  $$O(R*C)$$.
<br />
<br />


---
#### Approach 2: Dynamic Programming

**Intuition and Algorithm**

Let `dp(r, c, todo)` be the number of paths starting from where we are (`r, c`), and given that `todo` is the set of empty squares we've yet to walk on.

We can use a similar approach to *Approach 1*, except we will memoize these states `(r, c, todo)` so as not to repeat work.


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

* Time Complexity:  $$O(R * C * 2^{R*C})$$, where $$R, C$$ are the number of rows and columns in the grid.

* Space Complexity:  $$O(R * C)$$.
<br />
<br />


---
Analysis written by: [@awice](https://leetcode.com/awice).
