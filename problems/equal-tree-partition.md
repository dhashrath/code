#### Equal Tree Partition

```java
class Solution {
    Stack<Integer> seen;
    public boolean checkEqualTree(TreeNode root) {
        seen = new Stack();
        int total = sum(root);
        seen.pop();
        if (total % 2 == 0)
            for (int s: seen)
                if (s == total / 2)
                    return true;
        return false;
    }

    public int sum(TreeNode node) {
        if (node == null) return 0;
        seen.push(sum(node.left) + sum(node.right) + node.val);
        return seen.peek();
    }
}```


```python
class Solution(object):
    def checkEqualTree(self, root):
        seen = []

        def sum_(node):
            if not node: return 0
            seen.append(sum_(node.left) + sum_(node.right) + node.val)
            return seen[-1]

        total = sum_(root)
        seen.pop()
        return total / 2.0 in seen```

