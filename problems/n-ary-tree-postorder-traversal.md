#### N-ary Tree Postorder Traversal

```java
// Definition for a Node.
class Node {
  public int val;
  public List<Node> children;

  public Node() {}

  public Node(int _val, List<Node> _children) {
    val = _val;
    children = _children;
  }
};```


```python
# Definition for a Node.
class Node(object):
    def __init__(self, val, children):
        self.val = val
        self.children = children```


```java
class Solution {
  public List<Integer> postorder(Node root) {
    LinkedList<Node> stack = new LinkedList<>();
    LinkedList<Integer> output = new LinkedList<>();
    if (root == null) {
      return output;
    }

    stack.add(root);
    while (!stack.isEmpty()) {
      Node node = stack.pollLast();
      output.addFirst(node.val);
      for (Node item : node.children) {
        if (item != null) {
          stack.add(item);    
        } 
      }
    }
    return output;
  }
}```


```python
class Solution(object):
    def postorder(self, root):
        """
        :type root: Node
        :rtype: List[int]
        """
        if root is None:
            return []
        
        stack, output = [root, ], []
        while stack:
            root = stack.pop()
            if root is not None:
                output.append(root.val)
            for c in root.children:
                stack.append(c)
                
        return output[::-1]```

