

## Solution
---

#### Approach 1: Sort by Row

**Intuition**

By iterating through the string from left to right, we can easily determine which row in the Zig-Zag pattern that a character belongs to.

**Algorithm**

We can use $$\text{min}( \text{numRows}, \text{len}(s))$$ lists to represent the non-empty rows of the Zig-Zag Pattern.

Iterate through $$s$$ from left to right, appending each character to the appropriate row. The appropriate row can be tracked using two variables: the current row and the current direction.

The current direction changes only when we moved up to the topmost row or moved down to the bottommost row.


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

* Time Complexity: $$O(n)$$, where $$n == \text{len}(s)$$
* Space Complexity: $$O(n)$$

<br/>

---

#### Approach 2: Visit by Row

**Intuition**

Visit the characters in the same order as reading the Zig-Zag pattern line by line.

**Algorithm**

Visit all characters in row 0 first, then row 1, then row 2, and so on...

For all whole numbers $$k$$,

- Characters in row $$0$$ are located at indexes $$k \; (2 \cdot \text{numRows} - 2)$$
- Characters in row $$\text{numRows}-1$$ are located at indexes $$k \; (2 \cdot \text{numRows} - 2) + \text{numRows} - 1$$
- Characters in inner row $$i$$ are located at indexes $$k \; (2 \cdot \text{numRows}-2)+i$$ and $$(k+1)(2 \cdot \text{numRows}-2)- i$$.


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

* Time Complexity: $$O(n)$$, where $$n == \text{len}(s)$$. Each index is visited once.
* Space Complexity: $$O(n)$$. For the cpp implementation, $$O(1)$$ if return string is not considered extra space.
