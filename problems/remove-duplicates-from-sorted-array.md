

## Solution

---
#### Approach 1: Two Pointers

**Algorithm**

Since the array is already sorted, we can keep two pointers $$i$$ and $$j$$, where $$i$$ is the slow-runner while $$j$$ is the fast-runner. As long as $$nums[i] = nums[j]$$, we increment $$j$$ to skip the duplicate.

When we encounter $$nums[j] \neq nums[i]$$, the duplicate run has ended so we must copy its value to $$nums[i + 1]$$. $$i$$ is then incremented and we repeat the same process again until $$j$$ reaches the end of array.


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

* Time complextiy : $$O(n)$$.
Assume that $$n$$ is the length of array. Each of $$i$$ and $$j$$ traverses at most $$n$$ steps.

* Space complexity : $$O(1)$$.
