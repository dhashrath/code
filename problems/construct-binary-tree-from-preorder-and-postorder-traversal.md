

## Solution
---
#### Approach 1: Recursion

**Intuition**

A preorder traversal is:

* `(root node) (preorder of left branch) (preorder of right branch)`

While a postorder traversal is:

* `(postorder of left branch) (postorder of right branch) (root node)`

For example, if the final binary tree is `[1, 2, 3, 4, 5, 6, 7]` (serialized), then the preorder traversal is `[1] + [2, 4, 5] + [3, 6, 7]`, while the postorder traversal is `[4, 5, 2] + [6, 7, 3] + [1]`.

If we knew how many nodes the left branch had, we could partition these arrays as such, and use recursion to generate each branch of the tree.

**Algorithm**

Let's say the left branch has $$L$$ nodes.  We know the head node of that left branch is `pre[1]`, but it also occurs last in the postorder representation of the left branch.  So `pre[1] = post[L-1]` (because of uniqueness of the node values.)  Hence, `L = post.indexOf(pre[1]) + 1`.

Now in our recursion step, the left branch is represnted by `pre[1 : L+1]` and `post[0 : L]`, while the right branch is represented by `pre[L+1 : N]` and `post[L : N-1]`.


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

* Time Complexity:  $$O(N^2)$$, where $$N$$ is the number of nodes.

* Space Complexity:  $$O(N^2)$$.
<br />
<br />


---
#### Approach 2: Recursion (Space Saving Variant)

**Explanation**

We present a variation of *Approach 1* that uses indexes to refer to the subarrays of `pre` and `post`, instead of passing copies of those subarrays.  Here, `(i0, i1, N)` refers to `pre[i0:i0+N], post[i1:i1+N]`.


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

* Time Complexity:  $$O(N^2)$$, where $$N$$ is the number of nodes.

* Space Complexity:  $$O(N)$$, the space used by the answer.
<br />
<br />

---


Analysis written by: [@awice](https://leetcode.com/awice).
