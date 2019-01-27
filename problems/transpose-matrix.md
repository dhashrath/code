

## Solution
---
#### Approach 1: Copy Directly

**Intuition and Algorithm**

The transpose of a matrix `A` with dimensions `R x C` is a matrix `ans` with dimensions `C x R` for which `ans[c][r] = A[r][c]`.

Let's initialize a new matrix `ans` representing the answer.  Then, we'll copy each entry of the matrix as appropriate.


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

* Time Complexity:  $$O(R * C)$$, where $$R$$ and $$C$$ are the number of rows and columns in the given matrix `A`.

* Space Complexity:  $$O(R * C)$$, the space used by the answer.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
