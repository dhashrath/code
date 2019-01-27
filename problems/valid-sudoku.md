

## Solution

---

#### Intuition

The naive solution would be to iterate _three_ times over the board to ensure that :

* There is no rows with duplicates.
* There is no columns with duplicates.
* There is no sub-boxes with duplicates.

Actually, all this could be done in just one iteration. 

---
   
#### Approach 1: One iteration

Let's first discuss two questions.

* How to enumerate sub-boxes?

> One could use `box_index = (row / 3) * 3 + columns / 3` where
`/` is an integer division.

<img src="../Figures/36/36_boxes_2.png" width="500">

* How to ensure that there is no duplicates in a row / column / box?

> One could just track all values which were already encountered
in a hash map `value -> count`.

Now everything is ready for the overall algorithm :

* Move along the board. 
   * Check for each cell value if it was seen already in the current
   row / column / box :
      * Return `false` if yes.
      * Keep this value for a further tracking if no. 
* Return `true`.

!?!../Documents/36_LIS.json:1000,616!?!


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

* Time complexity : $$\mathcal{O}(1)$$ since all we do here is just one 
iteration over the board with `81` cells. 
* Space complexity : $$\mathcal{O}(1)$$. 


Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
