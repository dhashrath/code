

#### Approach #1: Prefix Sum [Accepted]

**Intuition and Algorithm**

We need to quickly compute the sum of values to the left and the right of every index.

Let's say we knew `S` as the sum of the numbers, and we are at index `i`.  If we knew the sum of numbers `leftsum` that are to the left of index `i`, then the other sum to the right of the index would just be `S - nums[i] - leftsum`.  

As such, we only need to know about `leftsum` to check whether an index is a pivot index in constant time.  Let's do that: as we iterate through candidate indexes `i`, we will maintain the correct value of `leftsum`.


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

* Time Complexity: $$O(N)$$, where $$N$$ is the length of `nums`.

* Space Complexity: $$O(1)$$, the space used by `leftsum` and `S`.

---

Analysis written by: [@awice](https://leetcode.com/awice).
