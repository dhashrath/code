

## Solution

---

#### Approach 1: Iterations

**Algorithm**

First of all, please refer to [this article](https://leetcode.com/articles/binary-tree-preorder-transversal/) 
for the solution in case of binary tree.
This article offers the same ideas with a bit of generalisation. 

First of all, here is the definition of the tree ```Node``` which we would use
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
In the implemented strategy we push nodes into output list 
following the order ```Top->Bottom``` and ```Left->Right```, that
naturally reproduces preorder traversal.


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
 and for each visit, the complexity of the operation
 (*i.e.* appending the child nodes) is
 proportional to the number of child nodes ```n``` (n-ary tree).
 Therefore the overall time complexity is $$\mathcal{O}(N)$$,
where $$N$$ is the number of nodes, *i.e.* the size of tree.

* Space complexity : depending on the tree structure, 
we could keep up to the entire tree, therefore, the space complexity is $$\mathcal{O}(N)$$.

Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
