

## Summary

This is a pretty easy problem, but one may get confused by the term "in-place" and thought it is impossible to remove an element from the array without making a copy of the array.

## Hints

1. Try two pointers.
2. Did you use the property of "the order of elements can be changed"?
3. What happens when the elements to remove are rare?

## Solution
---
#### Approach 1: Two Pointers

**Intuition**

Since question asked us to remove all elements of the given value in-place, we have to handle it with $$O(1)$$ extra space. How to solve it? We can keep two pointers $$i$$ and $$j$$, where $$i$$ is the slow-runner while $$j$$ is the fast-runner.

**Algorithm**

When $$nums[j]$$ equals to the given value, skip this element by incrementing $$j$$. As long as $$nums[j] \neq val$$, we copy $$nums[j]$$ to $$nums[i]$$ and increment both indexes at the same time. Repeat the process until $$j$$ reaches the end of the array and the new length is $$i$$.

This solution is very similar to the solution to [Remove Duplicates from Sorted Array](https://leetcode.com/articles/remove-duplicates-from-sorted-array/).


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

**Complexity analysis**

* Time complexity : $$O(n)$$.
Assume the array has a total of $$n$$ elements, both $$i$$ and $$j$$ traverse at most $$2n$$ steps.

* Space complexity : $$O(1)$$.
<br />
<br />
---

#### Approach 2: Two Pointers - when elements to remove are rare

**Intuition**

Now consider cases where the array contains few elements to remove. For example, $$nums = [1,2,3,5,4], val = 4$$. The previous algorithm will do unnecessary copy operation of the first four elements. Another example is $$nums = [4,1,2,3,5], val = 4$$. It seems unnecessary to move elements $$[1,2,3,5]$$ one step left as the problem description mentions that the order of elements could be changed.

**Algorithm**

When we encounter $$nums[i] = val$$, we can swap the current element out with the last element and dispose the last one. This essentially reduces the array's size by 1.

Note that the last element that was swapped in could be the value you want to remove itself. But don't worry, in the next iteration we will still check this element.


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


**Complexity analysis**

* Time complexity : $$O(n)$$.
Both $$i$$ and $$n$$ traverse at most $$n$$ steps. In this approach, the number of assignment operation is equal to the number of elements to remove. So it is more efficient if elements to remove are rare.

* Space complexity : $$O(1)$$.
