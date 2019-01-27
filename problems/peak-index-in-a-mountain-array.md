

---
#### Approach 1: Linear Scan

**Intuition and Algorithm**

The mountain increases until it doesn't.  The point at which it stops increasing is the peak.


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


---
#### Approach 2: Binary Search

**Intuition and Algorithm**

The comparison `A[i] < A[i+1]` in a mountain array looks like `[True, True, True, ..., True, False, False, ..., False]`: 1 or more boolean `True`s, followed by 1 or more boolean `False`.  For example, in the mountain array `[1, 2, 3, 4, 1]`, the comparisons `A[i] < A[i+1]` would be `True, True, True, False`.

We can binary search over this array of comparisons, to find the largest index `i` such that `A[i] < A[i+1]`.  For more on *binary search*, see the [LeetCode explore topic here.](https://leetcode.com/explore/learn/card/binary-search/)


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

* Time Complexity:  $$O(\log N)$$, where $$N$$ is the length of `A`.

* Space Complexity:  $$O(1)$$.


---

Analysis written by: [@awice](https://leetcode.com/awice).
