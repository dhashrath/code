

## Solution
---
#### Approach 1: Sort by Column

**Intuition**

Count each rectangle by right-most edge.

**Algorithm**

Group the points by `x` coordinates, so that we have columns of points.  Then, for every pair of points in a column (with coordinates `(x,y1)` and `(x,y2)`), check for the smallest rectangle with this pair of points as the rightmost edge.  We can do this by keeping memory of what pairs of points we've seen before.


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

* Time Complexity:  $$O(N^2)$$, where $$N$$ is the length of `points`.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---
#### Approach 2: Count by Diagonal

**Intuition**

For each pair of points in the array, consider them to be the long diagonal of a potential rectangle.  We can check if all 4 points are there using a Set.

For example, if the points are `(1, 1)` and `(5, 5)`, we check if we also have `(1, 5)` and `(5, 1)`.  If we do, we have a candidate rectangle.

**Algorithm**

Put all the points in a set.  For each pair of points, if the associated rectangle are 4 distinct points all in the set, then take the area of this rectangle as a candidate answer.


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

* Time Complexity:  $$O(N^2)$$, where $$N$$ is the length of `points`.

* Space Complexity:  $$O(N)$$, where $$H$$ is the height of the tree.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).  Approach #1 inspired by: [@lee215](https://leetcode.com/lee215).
