

## Solution

---

#### How to traverse the tree

There are two general strategies to traverse a tree:

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

![postorder](../Figures/145_transverse.png)

Here the problem is to construct a binary tree from its preorder and
inorder traversal.
<br />
<br />


---
#### Approach 1: Recursion

**Tree definition**

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

 
**Algorithm**

As discussed above the preorder traversal follows `Root -> Left -> Right` order,
that makes it very convenient to construct the tree from its root.

Let's do it. The first element in the *preorder* list is a root. 
This root splits *inorder* list into left and right subtrees.
Now one have to pop up the root from preorder list since 
it's already used as a tree node and then repeat the step above for the
left and right subtrees. 

<!--![LIS](../Figures/105/105_tr.gif)-->
!?!../Documents/105_LIS.json:1008,542!?!


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


**Complexity analysis**

* Time complexity : the popleft operation is cheap $$\mathcal{O}(1)$$,
but search by index operation in the inorder list requires up to $$N$$ operations
and the construction of inorder lists for the 
left and right subtrees requires $$N - 1$$ operations, where `N` is a number of
nodes in the tree.
For instance, for the first call of the function, the maximum cost is $$N + N - 1$$, 
for the second $$N - 1 + N - 2$$, and so on and so forth. 
Now let's compute the number of operations for the worst case of completely unbalanced tree : 
$$N + (N - 1) + (N - 1) + (N - 2) + ... + 1$$, therefore the time complexity is $$\mathcal{O}(N^2)$$. 

* Space complexity : $$\mathcal{O}(N)$$, since we store the entire tree.
 
Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
