#### Binary Tree Cameras

```java
class Solution {
    public int minCameraCover(TreeNode root) {
        int[] ans = solve(root);
        return Math.min(ans[1], ans[2]);
    }

    // 0: Strict ST; All nodes below this are covered, but not this one
    // 1: Normal ST; All nodes below and incl this are covered - no camera
    // 2: Placed camera; All nodes below this are covered, plus camera here
    public int[] solve(TreeNode node) {
        if (node == null)
            return new int[]{0, 0, 99999};

        int[] L = solve(node.left);
        int[] R = solve(node.right);
        int mL12 = Math.min(L[1], L[2]);
        int mR12 = Math.min(R[1], R[2]);

        int d0 = L[1] + R[1];
        int d1 = Math.min(L[2] + mR12, R[2] + mL12);
        int d2 = 1 + Math.min(L[0], mL12) + Math.min(R[0], mR12);
        return new int[]{d0, d1, d2};
    }
}
```


```python
class Solution(object):
    def minCameraCover(self, root):
        def solve(node):
            # 0: Strict ST; All nodes below this are covered, but not this one
            # 1: Normal ST; All nodes below and incl this are covered - no camera
            # 2: Placed camera; All nodes below this are covered, plus camera here

            if not node: return 0, 0, float('inf')
            L = solve(node.left)
            R = solve(node.right)

            dp0 = L[1] + R[1]
            dp1 = min(L[2] + min(R[1:]), R[2] + min(L[1:]))
            dp2 = 1 + min(L) + min(R)

            return dp0, dp1, dp2

        return min(solve(root)[1:])```


```java
class Solution {
    int ans;
    Set<TreeNode> covered;
    public int minCameraCover(TreeNode root) {
        ans = 0;
        covered = new HashSet();
        covered.add(null);

        dfs(root, null);
        return ans;
    }

    public void dfs(TreeNode node, TreeNode par) {
        if (node != null) {
            dfs(node.left, node);
            dfs(node.right, node);

            if (par == null && !covered.contains(node) ||
                    !covered.contains(node.left) ||
                    !covered.contains(node.right)) {
                ans++;
                covered.add(node);
                covered.add(par);
                covered.add(node.left);
                covered.add(node.right);
            }
        }
    }
}```


```python
class Solution(object):
    def minCameraCover(self, root):
        self.ans = 0
        covered = {None}

        def dfs(node, par = None):
            if node:
                dfs(node.left, node)
                dfs(node.right, node)

                if (par is None and node is not covered or
                        node.left not in covered or node.right not in covered):
                    self.ans += 1
                    covered.update({node, par, node.left, node.right})

        dfs(root)
        return self.ans```

