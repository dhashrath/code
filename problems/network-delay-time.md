

#### Approach #1: Depth-First Search [Accepted]

**Intuition**

Let's record the time `dist[node]` when the signal reaches the node.  If some signal arrived earlier, we don't need to broadcast it anymore.  Otherwise, we should broadcast the signal.

**Algorithm**

We'll maintain `dist[node]`, the earliest that we arrived at each `node`.  When visiting a `node` while `elapsed` time has elapsed, if this is the currently-fastest signal at this node, let's broadcast signals from this node.

To speed things up, at each visited node we'll consider signals exiting the node that are faster first, by sorting the edges.



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

* Time Complexity: $$O(N^N + E \log E)$$ where $$E$$ is the length of `times`.  We can only fully visit each node up to $$N-1$$ times, one per each other node.  Plus, we have to explore every edge and sort them.  Sorting each small bucket of outgoing edges is bounded by sorting all of them, because of repeated use of the inequality $$x \log x + y \log y \leq (x+y) \log (x+y)$$.

* Space Complexity: $$O(N + E)$$, the size of the graph ($$O(E)$$), plus the size of the implicit call stack in our DFS ($$O(N)$$).

---
#### Approach #2: Dijkstra's Algorithm [Accepted]

**Intuition and Algorithm**

We use *Dijkstra's algorithm* to find the shortest path from our source to all targets.  This is a textbook algorithm, refer to [this link](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm) for more details.

Dijkstra's algorithm is based on repeatedly making the candidate move that has the least distance travelled.

In our implementations below, we showcase both $$O(N^2)$$ (basic) and $$O(N \log N)$$ (heap) approaches.

*Basic Implementation*

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


*Heap Implementation*


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

* Time Complexity: $$O(N^2 + E)$$m where $$E$$ is the length of `times` in the basic implementation, and $$O(E \log E)$$ in the heap implementation, as potentially every edge gets added to the heap.

* Space Complexity: $$O(N + E)$$, the size of the graph ($$O(E)$$), plus the size of the other objects used ($$O(N)$$).

---

Analysis written by: [@awice](https://leetcode.com/awice).
