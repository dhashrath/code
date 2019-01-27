

#### Approach 1: Dynamic Programming

**Intuition**

If we have the a row of Pascal's triangle, we can easily compute the next
row by each pair of adjacent values.

**Algorithm**

Although the algorithm is very simple, the iterative approach to constructing
Pascal's triangle can be classified as dynamic programming because we
construct each row based on the previous row.

First, we generate the overall `triangle` list, which will store each row as
a sublist. Then, we check for the special case of $$0$$, as we would otherwise
return $$[1]$$. If $$numRows > 0$$, then we initialize `triangle` with $$[1]$$
as its first row, and proceed to fill the rows as follows:

!?!../Documents/118_Pascals_Triangle.json:1280,720!?!


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

* Time complexity : $$O(numRows^2)$$

    Although updating each value of `triangle` happens in constant time, it
    is performed $$O(numRows^2)$$ times. To see why, consider how many
    overall loop iterations there are. The outer loop obviously runs
    $$numRows$$ times, but for each iteration of the outer loop, the inner
    loop runs $$rowNum$$ times. Therefore, the overall number of `triangle` updates
    that occur is $$1 + 2 + 3 + \ldots + numRows$$, which, according to Gauss' formula,
    is

    $$
    \begin{aligned}
        \frac{numRows(numRows+1)}{2} &= \frac{numRows^2 + numRows}{2} \\
        &= \frac{numRows^2}{2} + \frac{numRows}{2} \\
        &= O(numRows^2)
    \end{aligned}
    $$

* Space complexity : $$O(numRows^2)$$

    Because we need to store each number that we update in `triangle`, the
    space requirement is the same as the time complexity.
