

## Solution

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

<br />
<br />


---
#### Approach 1: Recursion

**Algorithm**

<!--![LIS](../Figures/104/104_tr.gif)-->
!?!../Documents/104_LIS.json:1000,500!?!

The intuitive approach is to solve the problem by recursion.
Here we demonstrate an example with the DFS (Depth First Search) strategy. 


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

* Time complexity : we visit each node exactly once, 
thus the time complexity is $$\mathcal{O}(N)$$,
where $$N$$ is the number of nodes.

* Space complexity : in the worst case, the tree is completely unbalanced,
*e.g.* each node has only left child node, the recursion call would occur
 $$N$$ times (the height of the tree), therefore the storage to keep the call stack would be $$\mathcal{O}(N)$$.
 But in the best case (the tree is completely balanced), the height of the tree would be $$\log(N)$$.
 Therefore, the space complexity in this case would be $$\mathcal{O}(\log(N))$$.
<br />
<br />


---    
#### Approach 2: Iteration

We could also convert the above recursion into iteration, with the help of stack.

>The idea is to visit each node with the DFS strategy,
while updating the max depth at each visit.

So we start from a stack which contains the root node and the corresponding depth 
which is ```1```.
Then we proceed to the iterations: pop the current node out of the stack and
push the child nodes. The depth is updated at each step. 


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

* Time complexity : $$\mathcal{O}(N)$$.

* Space complexity : $$\mathcal{O}(N)$$.
 
Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
