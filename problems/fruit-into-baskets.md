

## Solution
---
#### Approach 1: Scan Through Blocks

**Intuition**

Equivalently, we want the longest subarray with at most two "types" (values of `tree[i]`).

Instead of considering each element individually, we can consider blocks of adjacent elements of the same type.

For example, instead of `tree = [1, 1, 1, 1, 2, 2, 3, 3, 3]`, we can say this is `blocks = [(1, weight = 4), (2, weight = 2), (3, weight = 3)]`.

Now say we brute forced, scanning from left to right.  We'll have something like `blocks = [1, _2_, 1, 2, 1, 2, _1_, 3, ...]` (with various weights).

The key insight is that when we encounter a `3`, we do not need to start from the second element `2` (marked `_2_` for convenience); we can start from the first element (`_1_`) before the `3`.  This is because if we started two or more elements before, the sequence must have types `1` and `2`, and that sequence is going to end at the `3`, and thus be shorter than anything we've already considered.

Since every starting point (that is the left-most index of a block) was considered, this solution is correct.

**Algorithm**

As the notation and strategy around implementing this differs between Python and Java, please see the inline comments for more details.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `tree`.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---
#### Approach 2: Sliding Window

**Intuition**

As in *Approach 1*, we want the longest subarray with at most two different "types" (values of `tree[i]`).  Call these subarrays *valid*.

Say we consider all valid subarrays that end at index `j`.  There must be one with the smallest possible starting index `i`: lets say `opt(j) = i`.

Now the key idea is that `opt(j)` is a monotone increasing function.  This is because any subarray of a valid subarray is valid.

**Algorithm**

Let's perform a sliding window, keeping the loop invariant that `i` will be the smallest index for which `[i, j]` is a valid subarray.

We'll maintain `count`, the count of all the elements in the subarray.  This allows us to quickly query whether there are 3 types in the subarray or not.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `tree`.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---

Analysis written by: [@awice](https://leetcode.com/awice).
