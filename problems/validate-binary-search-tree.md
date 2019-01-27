

## Solution

--- 

#### Tree definition

First of all, here is the definition of the ```TreeNode``` which we would use.


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

<br />
<br />


---
#### Intuition

[Data structure heap](https://en.wikipedia.org/wiki/Heap_(data_structure))
usually implemented as a binary search tree. 
That makes BST related questions so popular for the interviews.

On the first sight, the problem is trivial. Let's traverse the tree
and check at each step if `node.right.val > node.val` and 
`node.left.val < node.val`. This approach would even work for some
trees 
![compute](../Figures/98/98_not_bst.png)

The problem is this approach will not work for all cases. 
Not only the right child should be larger than the node 
but all the 
elements in the right subtree. Here is an example :

![compute](../Figures/98/98_not_bst_3.png)

That means one should keep both upper 
and lower limits for each node while traversing the tree, 
and compare the node value not
with children values but with these limits.
<br />
<br />


---
#### Approach 1: Recursion

The idea above could be implemented as a recursion.
One compares the node value with its upper and lower limits
if they are available. Then one repeats the same 
step recursively for left and right subtrees. 

<!--![LIS](../Figures/98/98_tr.gif)-->
!?!../Documents/98_LIS.json:1000,462!?!


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

<br />
<br />


---
#### Approach 2: Iteration

The above recursion could be converted into iteration, 
with the help of stack. DFS would be better than BFS since 
it works faster here.


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

* Time complexity : $$\mathcal{O}(N)$$ since in both cases we visit each node exactly once. 
* Space complexity : $$\mathcal{O}(N)$$ for both approaches since we keep up to the 
entire tree.

Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
