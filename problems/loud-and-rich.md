

---
#### Approach #1: Cached Depth-First Search [Accepted]

**Intuition**

Consider the directed graph with edge `x -> y` if `y` is richer than `x`.

For each person `x`, we want the quietest person in the subtree at `x`.

**Algorithm**

Construct the graph described above, and say `dfs(person)` is the quietest person in the subtree at `person`.   Notice because the statements are logically consistent, the graph must be a DAG - a directed graph with no cycles.

Now `dfs(person)` is either `person`, or `min(dfs(child) for child in person)`.  That is to say, the quietest person in the subtree is either the `person` itself, or the quietest person in some subtree of a child of `person`.

We can cache values of `dfs(person)` as `answer[person]`, when performing our *post-order traversal* of the graph.  That way, we don't repeat work.  This technique reduces a quadratic time algorithm down to linear time.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the number of people.

* Space Complexity:  $$O(N)$$, the space used by the `answer`, and the implicit call stack of `dfs`.

---

Analysis written by: [@awice](https://leetcode.com/awice).
