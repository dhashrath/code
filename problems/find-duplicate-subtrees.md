

---
#### Approach #1: Depth-First Search [Accepted]

**Intuition**

We can serialize each subtree.  For example, the tree
```python
   1
  / \
 2   3
    / \
   4   5
```

can be represented as the serialization `1,2,#,#,3,4,#,#,5,#,#`, which is a unique representation of the tree.

**Algorithm**

Perform a depth-first search, where the recursive function returns the serialization of the tree.  At each node, record the result in a map, and analyze the map after to determine duplicate subtrees.


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

* Time Complexity: $$O(N^2)$$, where $$N$$ is the number of nodes in the tree.  We visit each node once, but each creation of `serial` may take $$O(N)$$ work.

* Space Complexity: $$O(N^2)$$, the size of `count`.

---
#### Approach #2: Unique Identifier [Accepted]

**Intuition**

Suppose we have a unique identifier for subtrees: two subtrees are the same if and only if they have the same id.

Then, for a node with left child id of `x` and right child id of `y`, `(node.val, x, y)` uniquely determines the tree.

**Algorithm**

If we have seen the triple `(node.val, x, y)` before, we can use the identifier we've remembered.  Otherwise, we'll create a new one.


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

* Time Complexity: $$O(N)$$, where $$N$$ is the number of nodes in the tree.  We visit each node once.

* Space Complexity: $$O(N)$$.  Every structure we use is using $$O(1)$$ storage per node.

---

Analysis written by: [@awice](https://leetcode.com/awice).  Approach #2 inspired by [@StefanPochmann](https://discuss.leetcode.com/topic/97625/o-n-time-and-space-lots-of-analysis).
