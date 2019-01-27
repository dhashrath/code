

## Solution

---

#### Intuition

The naive solution would be to brute-force, 
_i.e._ to check all possible positions for the dots 
and keep only the valid ones. In the worst case that 
means `11` possible positions and hence $$11 \times 10 \times 9
= 990$$ validations.

That could be optimized with the help of two conceptions.

> The first one is called _constrained programming_. 

That basically means
to put restrictions after each dot placement. 

If one already put a dot that leaves only `3` possibilities
for the next dot to be placed : after one digit, 
after two digits, or after three digits. The first dot has 
only `3` available slots as well.

That propagates _constraints_ 
and helps to reduce a number of combinations to consider.
Instead of $$990$$ combinations it's enough to 
check just $$3 \times 3 \times 3 = 27$$.

> The second one called _backtracking_. 

Let's imagine that one put one or two dots already and that left
no way to place the others to create a valid IP address. 
What to do? _To backtrack_. That means to come back,
to change the position of the previously placed dot and try 
to proceed again. If that would not work either, _backtrack_ again.
<br />
<br />


---
#### Approach 1: Backtracking (DFS)

Here is an algorithm for the backtrack function
`backtrack(prev_pos = -1, dots = 3)` which takes 
position of the previously placed dot `prev_pos`
and number of dots to place `dots` as arguments :

* Iterate over three available slots `curr_pos` to place a dot. 
   * Check if the segment from the previous dot to the current one is valid :
      * Yes :
        * Place the dot.
        * Check if all `3` dots are placed :
            * Yes :
                * Add the solution into the output list.
            * No :
                * Proceed to place next dots `backtrack(curr_pos, dots - 1)`.
        * Remove the last dot to backtrack. 

!?!../Documents/93_LIS.json:1000,612!?!


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

* Time complexity : as discussed above, there is not more than `27`
combinations to check. 
* Space complexity : constant space to keep the solutions, not more 
than `19` valid IP addresses. 

<img src="../Figures/93/93_max_number.png" width="500">


Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
