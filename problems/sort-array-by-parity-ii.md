

## Solution
---
#### Approach 1: Two Pass

**Intuition and Algorithm**

Read all the even integers and put them into places `ans[0]`, `ans[2]`, `ans[4]`, and so on.

Then, read all the odd integers and put them into places `ans[1]`, `ans[3]`, `ans[5]`, etc.


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

* Space Complexity:  $$O(N)$$.
<br />
<br />


---
#### Approach 2: Read / Write Heads

**Intuition**

We are motivated (perhaps by the interviewer) to pursue a solution where we modify the original array `A` in place.

First, it is enough to put all even elements in the correct place, since all odd elements will be in the correct place too.  So let's only focus on `A[0], A[2], A[4], ...`

Ideally, we would like to have some partition where everything to the left is already correct, and everything to the right is undecided.

Indeed, this idea works if we separate it into two slices `even = A[0], A[2], A[4], ...` and `odd = A[1], A[3], A[5], ...`.  Our invariant will be that everything less than `i` in the even slice is correct, and everything less than `j` in the odd slice is correct.

**Algorithm**

For each even `i`, let's make `A[i]` even.  To do it, we will draft an element from the odd slice.  We pass `j` through the odd slice until we find an even element, then swap.  Our invariant is maintained, so the algorithm is correct.


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
