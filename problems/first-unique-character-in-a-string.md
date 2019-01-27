

## Solution

---

#### Approach 1: Linear time solution

The best possible solution here could be of a linear time 
because to ensure 
that the character is unique 
you have to check the whole string anyway. 

The idea is to go through the string and 
save in a hash map the number of times 
each character appears in the string. 
That would take $$\mathcal{O}(N)$$ time, 
where `N` is a number of characters in the string.
 
And then we go through the string the second time, this time 
we use the hash map as a reference to check if a character 
is unique or not.   
If the character is unique, one could just return its index. 
The complexity of the second iteration is $$\mathcal{O}(N)$$ as well.

<!--![LIS](../Figures/387/387_tr.gif)-->
!?!../Documents/387_LIS.json:1000,621!?!


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

* Time complexity : $$\mathcal{O}(N)$$ since we go 
through the string of length `N` two times. 
* Space complexity : $$\mathcal{O}(N)$$ since we have to keep a hash map 
with `N` elements.

Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
