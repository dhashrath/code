
#### Approach #1: Direct [Accepted]
**Intuition and Approach**

For each number from `L` to `R`, let's find out how many set bits it has.  If that number is `2, 3, 5, 7, 11, 13, 17`, or `19`, then we add one to our count.  We only need primes up to 19 because $$R \leq 10^6 < 2^{20}$$.


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
    
* Time Complexity: $$O(D)$$, where $$D = R-L$$ is the number of integers considered.  In a bit complexity model, this would be $$O(D\log D)$$ as we have to count the bits in $$O(\log D)$$ time.

* Space Complexity: $$O(1)$$.

---
Analysis written by: [@awice](https://leetcode.com/awice).
