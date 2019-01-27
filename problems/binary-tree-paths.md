

## Solution

---

#### Binary tree definition

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


#### Approach 1: Recursion

The most intuitive way is to use a recursion here.
One is going through the tree 
by considering at each step the node itself and its children.
If node *is not* a leaf, one extends the current path by a node value and
calls recursively the path construction for its children.
If node *is* a leaf, one closes the current path and adds it into 
the list of paths.


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

* Time complexity : we visit each node exactly once, 
thus the time complexity is $$\mathcal{O}(N)$$,
where $$N$$ is the number of nodes.
* Space complexity : $$\mathcal{O}(N)$$. Here we use the space for a stack call and for a 
`paths` list to store the answer. `paths` contains as many elements as leafs in the tree and 
hence couldn't be larger than $$log(N)$$ for the trees containing more than one element. 
Hence the space complexity is determined by a stack call.
In the worst case, when the tree is completely unbalanced,
*e.g.* each node has only one child node, the recursion call would occur
$$N$$ times (the height of the tree), therefore the storage to keep the call stack would be $$\mathcal{O}(N)$$.
But in the best case (the tree is balanced), the height of the tree would be $$\log(N)$$.
Therefore, the space complexity in this case would be $$\mathcal{O}(\log(N))$$.
<br/>
<br/>


---
#### Approach 2: Iterations

The approach above could be rewritten with the help of iterations. 
This way we initiate the stack by a root node and then at each step
we pop out one node and its path.
If the poped node *is* a leaf, one update the list of all paths.
If not, one pushes its child nodes and
corresponding paths into stack till all nodes are checked.

<!--![LIS](../Figures/257/257_tr.gif)-->
!?!../Documents/257_LIS.json:1000,491!?!


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

* Time complexity : $$\mathcal{O}(N)$$ since each node is visited exactly once. 
* Space complexity : $$\mathcal{O}(N)$$ as we could keep up to the entire tree.

Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
