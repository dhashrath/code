

## Solution
---
#### Approach 1: Iterate Triangles

**Intuition**

For each triangle, let's try to find the 4th point and whether it is a rectangle.

**Algorithm**

Say the first 3 points are `p1, p2, p3`, and that  `p2` and `p3` are opposite corners of the final rectangle.  The 4th point must be `p4 = p2 + p3 - p1` (using vector notation) because `p1, p2, p4, p3` must form a parallelogram, and `p1 + (p2 - p1) + (p3 - p1) = p4`.

If this point exists in our collection (we can use a `HashSet` to check), then we should check that the angles of this parallelogram are 90 degrees.  The easiest way is to check the dot product of the two vectors `(p2 - p1)` and `(p3 - p1)`.  (Another way is we could normalize the vectors to length 1, and check that one equals the other rotated by 90 degrees.)


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

* Time Complexity:  $$O(N^3)$$, where $$N$$ is the length of `points`.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---
#### Approach 2: Iterate Centers

**Intuition**

Consider opposite points `AC` and `BD` of a rectangle `ABCD`.  They both have the same center `O`, which is the midpoint of `AC` and the midpoint of `AB`; and they both have the same radius `dist(O, A) == dist(O, B) == dist(O, C) == dist(O, D)`.  Notice that a necessary and sufficient condition to form a rectangle with two opposite pairs of points is that the points must have the same center and radius.

Motivated by that result, let's classify each pair of points `PQ` by their center `C` = the midpoint of `PQ`, and the radius `r = dist(P, C)`.  Our strategy is to brute force on pairs of points with the same classification.

**Algorithm**

For each pair of points, classify them by `center` and `radius`.  We only need to record one of the points `P`, since the other point is `P' = 2 * center - P` (using vector notation).

For each `center` and `radius`, look at every possible rectangle (two pairs of points `P, P', Q, Q'`).  The area of this rectangle `dist(P, Q) * dist(P, Q')` is a candidate answer.


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

* Time Complexity:  $$O(N^2 \log N)$$, where $$N$$ is the length of `points`.  It can be shown that the number of pairs of points with the same classification is bounded by $$\log N$$ - [see this link for more.](https://en.wikipedia.org/wiki/Sum_of_squares_function#Particular_cases)

* Space Complexity:  $$O(N)$$.
<br />
<br />


---
Analysis written by: [@awice](https://leetcode.com/awice).
