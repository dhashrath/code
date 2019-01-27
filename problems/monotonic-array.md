

## Solution
---
#### Approach 1: Two Pass

**Intuition**

An array is *monotonic* if it is monotone increasing, or monotone decreasing.  Since `a <= b` and `b <= c` implies `a <= c`, we only need to check adjacent elements to determine if the array is monotone increasing (or decreasing, respectively).  We can check each of these properties in one pass.

**Algorithm**

To check whether an array `A` is monotone increasing, we'll check `A[i] <= A[i+1]` for all `i`.  The check for monotone decreasing is similar.


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
#### Approach 2: One Pass

**Intuition**

To perform this check in one pass, we want to handle a stream of comparisons from $$\{-1, 0, 1\}$$, corresponding to `<`, `==`, or `>`.  For example, with the array `[1, 2, 2, 3, 0]`, we will see the stream `(-1, 0, -1, 1)`.

**Algorithm**

Keep track of `store`, equal to the first non-zero comparison seen (if it exists.)  If we see the opposite comparison, the answer is `False`.

Otherwise, every comparison was (necessarily) in the set $$\{-1, 0\}$$, or every comparison was in the set $$\{0, 1\}$$, and therefore the array is monotonic.


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
#### Approach 3: One Pass (Simple Variant)

**Intuition and Algorithm**

To perform this check in one pass, we want to remember if it is monotone increasing or monotone decreasing.

It's monotone increasing if there aren't some adjacent values `A[i], A[i+1]` with `A[i] > A[i+1]`, and similarly for monotone decreasing.

If it is either monotone increasing or monotone decreasing, then `A` is monotonic.


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
