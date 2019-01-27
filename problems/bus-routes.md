

---
#### Approach #1: Breadth First Search [Accepted]

**Intuition**

Instead of thinking of the stops as nodes (of a graph), think of the buses as nodes.  We want to take the least number of buses, which is a shortest path problem, conducive to using a breadth-first search.

**Algorithm**

We perform a breadth first search on bus numbers.  When we start at `S`, originally we might be able to board many buses, and if we end at `T` we may have many `targets` for our goal state.

One difficulty is to efficiently decide whether two buses are connected by an edge.  They are connected if they share at least one bus stop.  Whether two lists share a common value can be done by set intersection (HashSet), or by sorting each list and using a two pointer approach.

To make our search easy, we will annotate the depth of each node: `info[0] = node, info[1] = depth`.


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

* Time Complexity:  Let $$N$$ denote the number of buses, and $$b_i$$ be the number of stops on the $$i$$th bus.

    * To create the graph, in Python we do $$O(\sum (N - i) b_i)$$ work (we can improve this by checking for which of `r1, r2` is smaller), while in Java we did a $$O(\sum b_i \log b_i)$$ sorting step, plus our searches are $$O(N \sum b_i)$$ work.

    * Our (breadth-first) search is on $$N$$ nodes, and each node could have $$N$$ edges, so it is $$O(N^2)$$.

* Space Complexity: $$O(N^2 + \sum b_i)$$ additional space complexity, the size of `graph` and `routes`.  In Java, our space complexity is $$O(N^2)$$ because we do not have an equivalent of `routes`.  Dual-pivot quicksort (as used in `Arrays.sort(int[])`) is an in-place algorithm, so in Java we did not increase our space complexity by sorting.

---

Analysis written by: [@awice](https://leetcode.com/awice).
