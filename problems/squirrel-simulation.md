

## Solution

---
#### Approach #1 Simple Solution [Accepted]

**Algorithm**

We know, the distance between any two points(tree, squirrel, nut) is given by the absolute difference between the corresponding x-coordinates and the corresponding y-coordinates. 

Now, in order to determine the required minimum distance, we need to observe a few points. Firstly, the order in which the nuts are picked doesn't affect the final result, except one of the nuts which needs to be visited first from the squirrel's starting position. For the rest of the nuts, it is mandatory to go from the tree to the nut and then come back as well. 

For the first visited nut, the saving obtained, given by $$d$$, is the difference between the distance between the tree and the current nut & the distance between the current nut and the squirrel. This is because for this nut, we need not travel from the tree to the nut, but need to travel an additional distance from the squirrel's original position to the nut.

While traversing over the $$nuts$$ array and adding the to-and-fro distance, we find out the saving, $$d$$, which can be obtained if the squirrel goes to the current nut first. Out of all the nuts, we find out the nut which maximizes the saving and then deduct this maximum saving from the sum total of the to-and-fro distance of all the nuts.

Note that the first nut to be picked needs not necessarily be the nut closest to the squirrel's start point, but it's the one which maximizes the savings.

![Squirrel_Nuts](../Figures/573_Squirrel.PNG)
{align="center"}


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

* Time complexity : $$O(n)$$. We need to traverse over the whole $$nuts$$ array once. $$n$$ refers to the size of $$nuts$$ array.

* Space complexity : $$O(1)$$. Constant space is used.

---
Analysis written by: [@vinod23](https://leetcode.com/vinod23)
