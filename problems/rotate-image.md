

## Solution

---

#### Intuition

To figure out the in-place solution let's first check 
how each element in the angle moves during the rotation. 
 
![compute](../Figures/48/48_angles.png)

That gives us an idea to split a given matrix in four rectangles and
reduce the initial problem to the rotation of these rectangles.

![compute](../Figures/48/48_rectangles.png) 

Now the solution is quite straightforward - 
one could move across the elements 
in the first rectangle and rotate them using a temp list of `4` elements.

<!--![LIS](../Figures/48/48_tr.gif)-->
!?!../Documents/48_LIS.json:1000,513!?!

#### Approach 1

Let's implement the ideas above.


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

* Time complexity : $$\mathcal{O}(N^2)$$ is a complexity given by two inserted loops. 
* Space complexity : $$\mathcal{O}(1)$$ since we do a rotation *in place* 
and allocate only the list of `4` elements as a temporary helper.

Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
