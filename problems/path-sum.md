

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

<br />
<br />


---
#### Approach 1: Recursion

The most intuitive way is to use a recursion here.
One is going through the tree 
by considering at each step the node itself and its children.
If node *is not* a leaf, one calls recursively `hasPathSum` method 
for its children with a sum decreased by the current node value.
If node *is* a leaf, one checks if the the current sum is zero, *i.e* 
if the initial sum was discovered.


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
* Space complexity : in the worst case, the tree is completely unbalanced,
*e.g.* each node has only one child node, the recursion call would occur
 $$N$$ times (the height of the tree), therefore the storage to keep the call stack would be $$\mathcal{O}(N)$$.
 But in the best case (the tree is completely balanced), the height of the tree would be $$\log(N)$$.
 Therefore, the space complexity in this case would be $$\mathcal{O}(\log(N))$$.
<br />
<br />


---
#### Approach 2: Iterations

**Algorithm**

We could also convert the above recursion into iteration, 
with the help of stack. DFS would be better than BFS here since 
it works faster except the worst case. In the worst case the path `root->leaf` 
with the given sum is the last considered one and in this case DFS results in
the same productivity as BFS. 

>The idea is to visit each node with the DFS strategy,
while updating the remaining sum to cumulate at each visit.

So we start from a stack which contains the root node and the corresponding 
remaining sum which is ```sum - root.val```.
Then we proceed to the iterations: pop the current node out of the stack 
and return ```True``` if the remaining sum is `0` and we're on the leaf node.
If the remaining sum is not zero or we're not on the leaf yet 
then we push the child nodes 
and corresponding remaining sums into stack.  

<!--![LIS](../Figures/112/112_tr.gif)-->
!?!../Documents/112_LIS.json:1000,543!?!


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

* Time complexity : the same as the recursion approach $$\mathcal{O}(N)$$.
* Space complexity : $$\mathcal{O}(N)$$ since in the worst case, when the tree is completely unbalanced,
*e.g.* each node has only one child node, we would keep all $$N$$ nodes in the stack.
 But in the best case (the tree is balanced), the height of the tree would be $$\log(N)$$.
 Therefore, the space complexity in this case would be $$\mathcal{O}(\log(N))$$.

Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
