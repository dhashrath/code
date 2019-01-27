

## Solution
---
#### Approach 1: Depth-First Search

**Intuition**

Let's say two stones are connected by an edge if they share a row or column, and define a connected component in the usual way for graphs: a subset of stones so that there doesn't exist an edge from a stone in the subset to a stone not in the subset.  For convenience, we refer to a *component* as meaning a connected component.

The main insight is that we can always make moves that reduce the number of stones in each component to 1.

Firstly, every stone belongs to exactly one component, and moves in one component do not affect another component.

Now, consider a spanning tree of our component.  We can make moves repeatedly from the leaves of this tree until there is one stone left.

**Algorithm**

To count connected components of the above graph, we will use depth-first search.

For every stone not yet visited, we will visit it and any stone in the same connected component.  Our depth-first search traverses each node in the component.

For each component, the answer changes by `-1 + component.size`.


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

* Time Complexity:  $$O(N^2)$$, where $$N$$ is the length of `stones`.

* Space Complexity:  $$O(N^2)$$.
<br />
<br />


---
#### Approach 2: Union-Find

**Intuition**

As in *Approach 1*, we will need to consider components of an underlying graph.  A "Disjoint Set Union" (DSU) data structure is ideal for this.

We will skip the explanation of how a DSU structure is implemented.  Please refer to [https://leetcode.com/problems/redundant-connection/solution/](https://leetcode.com/problems/redundant-connection/solution/) for a tutorial on DSU.

**Algorithm**

Let's connect row `i` to column `j`, which will be represented by `j+10000`.  The answer is the number of components after making all the connections.

Note that for brevity, our `DSU` implementation does not use union-by-rank.  This makes the asymptotic time complexity larger.


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

* Time Complexity:  $$O(N \log N)$$, where $$N$$ is the length of `stones`.  (If we used union-by-rank, this can be $$O(N * \alpha(N))$$, where $$\alpha$$ is the Inverse-Ackermann function.)

* Space Complexity:  $$O(N)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
