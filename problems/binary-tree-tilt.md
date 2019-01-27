


## Solution

---
#### Approach #1 Using Recursion [Accepted]

**Algorithm**

From the problem statement, it is clear that we need to find the tilt value at every node of the given tree and add up all the tilt values to obtain the final result. To find the tilt value at any node, we need to subtract the sum of all the nodes in its left subtree and the sum of all the nodes in its right subtree. 

Thus, to find the solution, we make use of a recursive function `traverse` which when called from any node, returns the sum of the nodes below the current node including itself. With the help of such sum values for the right and left subchild of any node, we can directly obtain the tilt value corresponding to that node.

The below animation depicts how the value passing and tilt calculation:

!?!../Documents/563_Binary.json:1000,563!?!



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

* Time complexity : $$O(n)$$. where $$n$$ is the number of nodes. Each node is visited once.
* Space complexity : $$O(n)$$. In worst case when the tree is skewed depth of tree will be $$n$$. In average case depth will be $$logn$$.

---

Analysis written by: [@vinod23](https://leetcode.com/vinod23)
