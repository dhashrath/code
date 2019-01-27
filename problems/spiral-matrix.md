

#### Approach 1: Simulation

**Intuition**

Draw the path that the spiral makes.  We know that the path should turn clockwise whenever it would go out of bounds or into a cell that was previously visited.

**Algorithm**

Let the array have $$\text{R}$$ rows and $$\text{C}$$ columns.  $$\text{seen[r][c]}$$ denotes that the cell on the$$\text{r}$$-th row and $$\text{c}$$-th column was previously visited.  Our current position is $$\text{(r, c)}$$, facing direction $$\text{di}$$, and we want to visit $$\text{R}$$ x $$\text{C}$$ total cells.

As we move through the matrix, our candidate next position is $$\text{(cr, cc)}$$.  If the candidate is in the bounds of the matrix and unseen, then it becomes our next position; otherwise, our next position is the one after performing a clockwise turn.



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

* Time Complexity:  $$O(N)$$, where $$N$$ is the total number of elements in the input matrix.  We add every element in the matrix to our final answer.

* Space Complexity: $$O(N)$$, the information stored in `seen` and in `ans`.
<br />
<br />
---
#### Approach 2: Layer-by-Layer

**Intuition**

The answer will be all the elements in clockwise order from the first-outer layer, followed by the elements from the second-outer layer, and so on.

**Algorithm**

We define the $$\text{k}$$-th outer layer of a matrix as all elements that have minimum distance to some border equal to $$\text{k}$$.  For example, the following matrix has all elements in the first-outer layer equal to 1, all elements in the second-outer layer equal to 2, and all elements in the third-outer layer equal to 3.

```plain-text
[[1, 1, 1, 1, 1, 1, 1],
 [1, 2, 2, 2, 2, 2, 1],
 [1, 2, 3, 3, 3, 2, 1],
 [1, 2, 2, 2, 2, 2, 1],
 [1, 1, 1, 1, 1, 1, 1]]
```

For each outer layer, we want to iterate through its elements in clockwise order starting from the top left corner.  Suppose the current outer layer has top-left coordinates $$\text{(r1, c1)}$$ and bottom-right coordinates $$\text{(r2, c2)}$$.

Then, the top row is the set of elements $$\text{(r1, c)}$$ for $$\text{c = c1,...,c2}$$, in that order.  The rest of the right side is the set of elements $$\text{(r, c2)}$$ for $$\text{r = r1+1,...,r2}$$, in that order.  Then, if there are four sides to this layer (ie., $$\text{r1 < r2}$$ and $$\text{c1 < c2}$$), we iterate through the bottom side and left side as shown in the solutions below.

![SpiralMatrix](../Figures/54_spiralmatrix.png)


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the total number of elements in the input matrix.  We add every element in the matrix to our final answer.

* Space Complexity: $$O(N)$$, the information stored in `ans`.
