

## Solution

---

#### Approach 1: Backtracking

[Backtracking](https://en.wikipedia.org/wiki/Backtracking) 
is an algorithm for finding all
solutions by exploring all potential candidates.
If the solution candidate turns to be _not_ a solution 
(or at least not the _last_ one), 
backtracking algorithm discards it by making some changes 
on the previous step, *i.e.* _backtracks_ and then try again.

Here is a backtrack function 
which takes the index of the first integer to consider 
as an argument `backtrack(first)`.

* If the first integer to consider has index `n` that means 
that the current permutation is done.
* Iterate over the integers from index `first` to index `n - 1`.
    * Place `i`-th integer first in the permutation, 
    i.e. `swap(nums[first], nums[i])`.
    * Proceed to create all permutations which starts from 
    `i`-th integer : `backtrack(first + 1)`.
    * Now backtrack, i.e. `swap(nums[first], nums[i])` back.
        
!?!../Documents/46_LIS.json:1000,548!?!


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

* Time complexity : $$\mathcal{O}(N!)$$ to build `N!` solutions. 
* Space complexity : $$\mathcal{O}(N!)$$ since one has to keep
`N!` solutions.

Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
