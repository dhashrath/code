

## Solution

---
   
#### Approach 1: Binary search

The problem is to implement a search in $$\mathcal{O}(\log(N))$$ time
that gives an idea to use a binary search.

The algorithm is quite straightforward : 

* Find a rotation index `rotation_index`, 
_i.e._ index of the smallest element in the array.
Binary search works just perfect here.

* `rotation_index` splits array in two parts. 
Compare `nums[0]` and `target` 
to identify in which part one has to look for `target`.

* Perform a binary search in the chosen part of the array. 
        
!?!../Documents/33_LIS.json:1000,510!?!


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

* Time complexity : $$\mathcal{O}(\log(N))$$. 
* Space complexity : $$\mathcal{O}(1)$$. 


Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
