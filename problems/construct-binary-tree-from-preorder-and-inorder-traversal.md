#### Construct Binary Tree from Preorder and Inorder Traversal

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
import javafx.util.Pair;
class Solution {
  public Pair<TreeNode, int []> helper(int[] preorder, int[] inorder) {
    if (inorder.length == 0) {
      return new Pair(null, preorder);
    }

    // pick up the first element as a root
    int root_val = preorder[0];
    TreeNode root = new TreeNode(root_val);

    // find index of root in the inorder list
    int index = 0;
    for (; (index < inorder.length) && (inorder[index] != root_val); index++){}
    preorder = Arrays.copyOfRange(preorder, 1, preorder.length);

    // root splits inorder list
    // into left and right subtrees        
    int [] left_inorder = Arrays.copyOfRange(inorder, 0, index);
    int [] right_inorder = index + 1 <= inorder.length ? 
      Arrays.copyOfRange(inorder, index + 1, inorder.length) : new int [0];

    // recursion
    Pair<TreeNode, int []> p = helper(preorder, left_inorder);
    root.left = p.getKey();
    preorder = p.getValue();
    p = helper(preorder, right_inorder);
    root.right = p.getKey();
    preorder = p.getValue();

    return new Pair(root, preorder);
  }

  public TreeNode buildTree(int[] preorder, int[] inorder) {
    Pair<TreeNode, int []> result = helper(preorder, inorder);
    return result.getKey();
  }
}```


```python
from collections import deque
class Solution:
    def buildTree(self, preorder, inorder):
        """
        :type preorder: List[int]
        :type inorder: List[int]
        :rtype: TreeNode
        """
        def helper(preorder, inorder):
            if not inorder:
                return None
            
            # pick up the first element as a root
            root_val = preorder.popleft()
            root = TreeNode(root_val)

            # root splits inorder list
            # into left and right subtrees
            index = inorder.index(root_val)

            # recursion 
            root.left= helper(preorder, inorder[:index])
            root.right = helper(preorder, inorder[index + 1:])
            return root
        
        return helper(deque(preorder), inorder)
```

