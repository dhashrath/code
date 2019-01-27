


---
#### Approach #1: Group by Category [Accepted]

**Intuition and Algorithm**

We ask what feature makes two coordinates `(r1, c1)` and `(r2, c2)` belong to the same diagonal?

It turns out two coordinates are on the same diagonal if and only if `r1 - c1 == r2 - c2`.

This leads to the following idea: remember the value of that diagonal as `groups[r-c]`.  If we see a mismatch, the matrix is not Toeplitz; otherwise it is.


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

* Time Complexity: $$O(M*N)$$.  (Recall in the problem statement that $$M, N$$ are the number of rows and columns in `matrix`.)

* Space Complexity: $$O(M+N)$$.

---
#### Approach #2: Compare With Top-Left Neighbor [Accepted]

**Intuition and Algorithm**

For each diagonal with elements in order $$a_1, a_2, a_3, \dots, a_k$$, we can check $$a_1 = a_2, a_2 = a_3, \dots, a_{k-1} = a_k$$.  The matrix is *Toeplitz* if and only if all of these conditions are true for all (top-left to bottom-right) diagonals.

Every element belongs to some diagonal, and it's previous element (if it exists) is it's top-left neighbor.  Thus, for the square `(r, c)`, we only need to check `r == 0 OR c == 0 OR matrix[r-1][c-1] == matrix[r][c]`.


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

* Time Complexity: $$O(M*N)$$, as defined in the problem statement.

* Space Complexity: $$O(1)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice).
