

#### Approach #1: Depth-First Search [Accepted]

**Intuition**

Starting from a rooted tree with `N-1` edges and `N` vertices, let's enumerate the possibilities for the added "redundant" edge.  If there is no loop, then either one vertex must have two parents (or no edge is redundant.)  If there is a loop, then either one vertex has two parents, or every vertex has one parent.

In the first two cases, there are only two candidates for deleting an edge, and we can try removing the last one and seeing if that works.  In the last case, the last edge of the cycle can be removed: for example, when `1->2->3->4->1->5`, we want the last edge (by order of occurrence) in the cycle `1->2->3->4->1` (but not necessarily `1->5`).

**Algorithm**

We'll first construct the underlying graph, keeping track of edges coming from nodes with multiple parents.  After, we either have 2 or 0 `candidates`.

If there are no candidates, then every vertex has one parent, such as in the case `1->2->3->4->1->5`.  From any node, we walk towards it's parent until we revisit a node - then we must be inside the cycle, and any future seen nodes are part of that cycle.  Now we take the last edge that occurs in the cycle.

Otherwise, we'll see if the graph induced by `parent` is a rooted tree.  We again take the `root` by walking from any node towards the parent until we can't, then we perform a depth-first search on this `root`.  If we visit every node, then removing the last of the two edge candidates is acceptable, and we should.  Otherwise, we should remove the first of the two edge candidates.

In our solution, we use `orbit` to find the result upon walking from a node `x` towards it's parent repeatedly until you revisit a node or can't walk anymore.  `orbit(x).node` (or `orbit(x)[0]` in Python) will be the resulting node, while `orbit(x).seen` (or `orbit(x)[1]`) will be all the nodes visited.


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

* Time Complexity:  $$O(N)$$ where $$N$$ is the number of vertices (and also the number of edges) in the graph.  We perform a depth-first search.

* Space Complexity:  $$O(N)$$, the size of the graph.

---

Analysis written by: [@awice](https://leetcode.com/awice)
