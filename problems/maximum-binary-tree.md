


## Solution

---
#### Approach #1 Recursive Solution[Accepted]

The current solution is very simple. We make use of a function `construct(nums, l, r)`, which returns the maximum binary tree consisting of numbers within the indices $$l$$ and $$r$$ in the given $$nums$$ array(excluding the $$r^{th}$$ element).

The algorithm consists of the following steps:

1. Start with the function call `construct(nums, 0, n)`. Here, $$n$$ refers to the number of elements in the given $$nums$$ array.

2. Find the index, $$max_i$$, of the largest element in the current range of indices $$(l:r-1)$$. Make this largest element, $$$nums[max_i]$$ as the local root node.

3. Determine the left child using `construct(nums, l, max_i)`. Doing this recursively finds the largest element in the subarray left to the current largest element.

4. Similarly, determine the right child using `construct(nums, max_i + 1, r)`.

5. Return the root node to the calling function.



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

* Time complexity : $$O(n^2)$$. The function `construct` is called $$n$$ times. At each level of the recursive tree, we traverse over all the $$n$$ elements to find the maximum element.  In the average case, there will be a $$log(n)$$ levels leading to a complexity of $$O\big(nlog(n)\big)$$. In the worst case, the depth of the recursive tree can grow upto $$n$$, which happens in the case of a sorted $$nums$$ array, giving a complexity of $$O(n^2)$$.

* Space complexity : $$O(n)$$. The size of the $$set$$ can grow upto $$n$$ in the worst case. In the average case, the size will be $$log(n)$$ for $$n$$ elements in $$nums$$, giving an average case complexity of $$O(log(n))$$

---
Analysis written by: [@vinod23](https://leetcode.com/vinod23)
