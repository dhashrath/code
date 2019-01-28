#### Validate Binary Search Tree

```java
// Definition for a binary tree node.
public class TreeNode {
  int val;
  TreeNode left;
  TreeNode right;

  TreeNode(int x) {
    val = x;
  }
}```


```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None```


```java
class Solution {
  public boolean isBSTHelper(TreeNode node, Integer lower_limit, Integer upper_limit) {
    if ((lower_limit != null) && (node.val <= lower_limit))
      return false;
    if ((upper_limit != null) && (upper_limit <= node.val))
      return false;

    boolean left = node.left != null ? isBSTHelper(node.left, lower_limit, node.val) : true;
    if (left) {
      boolean right = node.right != null ? isBSTHelper(node.right, node.val, upper_limit) : true;
      return right;
    } else
      return false;
  }

  public boolean isValidBST(TreeNode root) {
    if (root == null)
      return true;

    return isBSTHelper(root, null, null);
  }
}```


```python
class Solution:
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if not root:
            return True

        def isBSTHelper(node, lower_limit, upper_limit):
            if lower_limit is not None and node.val <= lower_limit:
                return False
            if upper_limit is not None and upper_limit <= node.val:
                return False

            left = isBSTHelper(node.left, lower_limit, node.val) if node.left else True
            if left:
                right = isBSTHelper(node.right, node.val, upper_limit) if node.right else True
                return right
            else:
                return False
        
        return isBSTHelper(root, None, None)```


```java
class Solution {
  public boolean isValidBST(TreeNode root) {
    if (root == null)
      return true;

    LinkedList<TreeNode> stack = new LinkedList();
    LinkedList<Integer> upper_limits = new LinkedList();
    LinkedList<Integer> lower_limits = new LinkedList();
    stack.add(root);
    upper_limits.add(null);
    lower_limits.add(null);

    while (!stack.isEmpty()) {
      TreeNode node = stack.poll();
      Integer lower_limit = lower_limits.poll();
      Integer upper_limit = upper_limits.poll();
      if (node.right != null) {
        if (node.right.val > node.val) {
          if ((upper_limit != null) && (node.right.val >= upper_limit))
            return false;
          stack.add(node.right);
          lower_limits.add(node.val);
          upper_limits.add(upper_limit);
        } else
          return false;
      }

      if (node.left != null) {
        if (node.left.val < node.val) {
          if ((lower_limit != null) && (node.left.val <= lower_limit))
            return false;
          stack.add(node.left);
          lower_limits.add(lower_limit);
          upper_limits.add(node.val);
        } else
          return false;
      }
    }
    return true;
  }
}```


```python
class Solution:
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if not root:
            return True
            
        stack = [(root, None, None), ] 
        while stack:
            root, lower_limit, upper_limit = stack.pop()
            if root.right:
                if root.right.val > root.val:
                    if upper_limit and root.right.val >= upper_limit:
                        return False
                    stack.append((root.right, root.val, upper_limit))
                else:
                    return False
            if root.left:
                if root.left.val < root.val:
                    if lower_limit and root.left.val <= lower_limit:
                        return False
                    stack.append((root.left, lower_limit, root.val))
                else:
                    return False
        return True  ```

