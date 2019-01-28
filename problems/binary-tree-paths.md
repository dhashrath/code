#### Binary Tree Paths

```java
/* Definition for a binary tree node. */
public class TreeNode {
  int val;
  TreeNode left;
  TreeNode right;

  TreeNode(int x) {
    val = x;
  }
}```


```python
class TreeNode(object):
    """ Definition of a binary tree node."""
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None```


```java
class Solution {
  public void construct_paths(TreeNode root, String path, LinkedList<String> paths) {
    if (root != null) {
      path += Integer.toString(root.val);
      if ((root.left == null) && (root.right == null))  // if reach a leaf
        paths.add(path);  // update paths
      else {
        path += "->";  // extend the current path
        construct_paths(root.left, path, paths);
        construct_paths(root.right, path, paths);
      }
    }
  }

  public List<String> binaryTreePaths(TreeNode root) {
    LinkedList<String> paths = new LinkedList();
    construct_paths(root, "", paths);
    return paths;
  }
}```


```python
class Solution:
    def binaryTreePaths(self, root):
        """
        :type root: TreeNode
        :rtype: List[str]
        """
        def construct_paths(root, path):
            if root:
                path += str(root.val)
                if not root.left and not root.right:  # if reach a leaf
                    paths.append(path)  # update paths  
                else:
                    path += '->'  # extend the current path
                    construct_paths(root.left, path)
                    construct_paths(root.right, path)

        paths = []
        construct_paths(root, '')
        return paths```


```java
class Solution {
  public List<String> binaryTreePaths(TreeNode root) {
    LinkedList<String> paths = new LinkedList();
    if (root == null)
      return paths;

    LinkedList<TreeNode> node_stack = new LinkedList();
    LinkedList<String> path_stack = new LinkedList();
    node_stack.add(root);
    path_stack.add(Integer.toString(root.val));
    TreeNode node;
    String path;
    while ( !node_stack.isEmpty() ) {
      node = node_stack.pollLast();
      path = path_stack.pollLast();
      if ((node.left == null) && (node.right == null))
        paths.add(path);
      if (node.left != null) {
        node_stack.add(node.left);
        path_stack.add(path + "->" + Integer.toString(node.left.val));
      }
      if (node.right != null) {
        node_stack.add(node.right);
        path_stack.add(path + "->" + Integer.toString(node.right.val));
      }
    }
    return paths;
  }
}```


```python
class Solution:
    def binaryTreePaths(self, root):
        """
        :type root: TreeNode
        :rtype: List[str]
        """
        if not root:
            return []
        
        paths = []
        stack = [(root, str(root.val))]
        while stack:
            node, path = stack.pop()
            if not node.left and not node.right:
                paths.append(path)
            if node.left:
                stack.append((node.left, path + '->' + str(node.left.val)))
            if node.right:
                stack.append((node.right, path + '->' + str(node.right.val)))
        
        return paths```

