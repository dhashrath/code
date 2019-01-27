

#### Approach #1: Depth-First Search [Accepted]

**Intuition and Algorithm**

After removing some edge from `parent` to `child`, (where the `child` cannot be the original `root`) the subtree rooted at `child` must be half the sum of the entire tree.

Let's record the sum of every subtree.  We can do this recursively using depth-first search.  After, we should check that half the sum of the entire tree occurs somewhere in our recording (and not from the total of the entire tree.)

Our careful treatment and analysis above prevented errors in the case of these trees:
```python
  0
 / \
-1  1

 0
  \
   0
```


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

* Time Complexity: $$O(N)$$ where $$N$$ is the number of nodes in the input tree.  We traverse every node.

* Space Complexity: $$O(N)$$, the size of `seen` and the implicit call stack in our DFS.

---

Analysis written by: [@awice](https://leetcode.com/awice).
