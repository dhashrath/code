

## Solution

---

#### How to transverse the tree


There are two general strategies to transverse a tree:

- *Breadth First Search* (`BFS`)

    We scan through the tree level by level, following the order of height,
    from top to bottom. The nodes on higher level would be visited before
    the ones with lower levels.
     
- *Depth First Search* (`DFS`)

    In this strategy, we adopt the `depth` as the priority, so that one
    would start from a root and reach all the way down to certain leaf,
    and then back to root to reach another branch.

    The DFS strategy can further be distinguished as
    `preorder`, `inorder`, and `postorder` depending on the relative order
    among the root node, left node and right node.
    
On the following figure the nodes are numerated in the order you visit them,
please follow ```1-2-3-4-5``` to compare different strategies.

<center><img src="../Figures/145_transverse.png" width="550px" /></center>

Here the problem is to implement postorder transversal using iterations.
<br />
<br />


---
#### Approach 1: Iterations

**Algorithm**

First of all, here is the definition of the ```TreeNode``` which we would use
in the following implementation.


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


Let's start from the root and then at each iteration 
pop the current node out of the stack and
push its child nodes. 
In the implemented strategy we push nodes into stack following the order ```Top->Bottom``` and ```Left->Right```.
Since DFS postorder transversal is ```Bottom->Top``` and ```Left->Right``` the output list
should be reverted after the end of loop.


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

* Time complexity : we visit each node exactly once, thus the 
time complexity is $$\mathcal{O}(N)$$,
where $$N$$ is the number of nodes, *i.e.* the size of tree.

* Space complexity : depending on the tree structure, 
we could keep up to the entire tree, therefore, the space complexity is $$\mathcal{O}(N)$$.

Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
