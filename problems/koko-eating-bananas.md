

## Solution
---
#### Approach 1: Binary Search

**Intuition**

If Koko can finish eating all the bananas (within `H` hours) with an eating speed of `K`, she can finish with a larger speed too.

If we let `possible(K)` be `true` if and only if Koko can finish with an eating speed of `K`, then there is some `X` such that `possible(K) = True` if and only if `K >= X`.

For example, with `piles = [3, 6, 7, 11]` and `H = 8`, there is some `X = 4` so that `possible(1) = possible(2) = possible(3) = False`, and `possible(4) = possible(5) = ... = True`.

**Algorithm**

We can binary search on the values of `possible(K)` to find the first `X` such that `possible(X)` is `True`: that will be our answer.  Our loop invariant will be that `possible(hi)` is always `True`, and `lo` is always less than or equal to the answer.  For more information on binary search, please visit [[LeetCode Explore - Binary Search]](https://leetcode.com/explore/learn/card/binary-search/).

To find the value of `possible(K)`, (ie. whether `Koko` with an eating speed of `K` can eat all bananas in `H` hours), we simulate it.  For each pile of size `p > 0`, we can deduce that Koko finishes it in `Math.ceil(p / K) = ((p-1) // K) + 1` hours, and we add these times across all piles and compare it to `H`.



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

* Time Complexity:  $$O(N \log W)$$, where $$N$$ is the number of piles, and $$W$$ is the maximum size of a pile.

* Space Complexity:  $$O(1)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
