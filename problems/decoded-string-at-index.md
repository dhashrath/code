

## Solution
---
#### Approach 1: Work Backwards

**Intuition**

If we have a decoded string like `appleappleappleappleappleapple` and an index like `K = 24`, the answer is the same if `K = 4`.

In general, when a decoded string is equal to some word with `size` length repeated some number of times (such as `apple` with `size = 5` repeated 6 times), the answer is the same for the index `K` as it is for the index `K % size`.

We can use this insight by working backwards, keeping track of the size of the decoded string.  Whenever the decoded string would equal some `word` repeated `d` times, we can reduce `K` to `K % (word.length)`.

**Algorithm**

First, find the length of the decoded string.  After, we'll work backwards, keeping track of `size`: the length of the decoded string after parsing symbols `S[0], S[1], ..., S[i]`.

If we see a digit `S[i]`, it means the size of the decoded string after parsing `S[0], S[1], ..., S[i-1]` will be `size / Integer(S[i])`.  Otherwise, it will be `size - 1`.



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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `S`.

* Space Complexity:  $$O(1)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
