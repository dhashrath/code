

## Solution

---

#### Approach 1: Divide and conquer 

Let's simplify the problem by representing it as a set of simple sub-problems.
One could split the initial integer `1234567890` on the groups 
containing not more than three digits `1.234.567.890`.
That results in representation `1 Billion 234 Million 567 Thousand 890` and 
reduces the initial problem to how to convert 3-digit integer to English word. 
One could split further `234` -> `2 Hundred 34` into two sub-problems :
convert 1-digit integer and convert 2-digit integer. The first one is trivial. 
The second one could be reduced to the first one for all 2-digit integers 
but the ones from `10` to `19` which should be considered separately.

<!--![LIS](../Figures/273/273_tr.gif)-->
!?!../Documents/273_LIS.json:1000,632!?!


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

* Time complexity :  $$\mathcal{O}(N)$$. 
Intuitively the output is proportional
to the number `N` of digits in the input. 
* Space complexity : $$\mathcal{O}(1)$$ since the output is just a string. 

Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
