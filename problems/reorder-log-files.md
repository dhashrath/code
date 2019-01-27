

## Solution
---
#### Approach 1: Custom Sort

**Intuition and Algorithm**

Instead of sorting in the default order, we'll sort in a custom order we specify.

The rules are:

* Letter-logs come before digit-logs;
* Letter-logs are sorted alphanumerically, by content then identifier;
* Digit-logs remain in the same order.

It is straightforward to translate these ideas into code.


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

* Time Complexity:  $$O(\mathcal{A}\log \mathcal{A})$$, where $$\mathcal{A}$$ is the total content of `logs`.

* Space Complexity:  $$O(\mathcal{A})$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
