

## Solution
---
#### Approach 1: Union-Find

**Intuition**

To find the number of components in a graph, we can use either depth-first search or union find.  The main difficulty with this problem is in specifying the graph.

One "brute force" way to specify the graph is to associate each grid square with 4 nodes (north, south, west, and east), representing 4 triangles inside the square if it were to have both slashes.  Then, we can connect all 4 nodes if the grid square is `" "`, and connect two pairs if the grid square is `"/"` or `"\"`.  Finally, we can connect all neighboring nodes (for example, the east node of the square at `grid[0][0]` connects with the west node of the square at `grid[0][1]`).

This is the most straightforward approach, but there are other approaches that use less nodes to represent the underlying information.

**Algorithm**

Create `4*N*N` nodes, one for each grid square, and connect them as described above.  After, we use a union find structure to find the number of connected components.

We will skip the explanation of how a DSU structure is implemented.  Please refer to [https://leetcode.com/problems/redundant-connection/solution/](https://leetcode.com/problems/redundant-connection/solution/) for a tutorial on DSU.


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

* Time Complexity:  $$O(N * N * \alpha(N))$$, where $$N$$ is the length of the grid, and $$\alpha$$ is the Inverse-Ackermann function (if we were to use union-find by rank.)

* Space Complexity:  $$O(N * N)$$.
<br />
<br />


---
Analysis written by: [@awice](https://leetcode.com/awice).
