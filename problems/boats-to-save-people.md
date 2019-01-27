

## Solution
---
#### Approach 1: Greedy (Two Pointer)

**Intuition**

If the heaviest person can share a boat with the lightest person, then do so.  Otherwise, the heaviest person can't pair with anyone, so they get their own boat.

The reason this works is because if the lightest person can pair with anyone, they might as well pair with the heaviest person.

**Algorithm**

Let `people[i]` to the currently lightest person, and `people[j]` to the heaviest.

Then, as described above, if the heaviest person can share a boat with the lightest person (if `people[j] + people[i] <= limit`) then do so; otherwise, the heaviest person sits in their own boat.


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

* Time Complexity:  $$O(N \log N)$$, where $$N$$ is the length of `people`.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
