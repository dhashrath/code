#### Path Sum IV

```java
class Solution {
    int ans = 0;
    public int pathSum(int[] nums) {
        Node root = new Node(nums[0] % 10);
        for (int num: nums) {
            if (num == nums[0]) continue;
            int depth = num / 100, pos = num / 10 % 10, val = num % 10;
            pos--;
            Node cur = root;
            for (int d = depth - 2; d >= 0; --d) {
                if (pos < 1<<d) {
                    if (cur.left == null) cur.left = new Node(val);
                    cur = cur.left;
                } else {
                    if (cur.right == null) cur.right = new Node(val);
                    cur = cur.right;
                }
                pos %= 1<<d;
            }
        }

        dfs(root, 0);
        return ans;
    }

    public void dfs(Node node, int sum) {
        if (node == null) return;
        sum += node.val;
        if (node.left == null && node.right == null) {
            ans += sum;
        } else {
            dfs(node.left, sum);
            dfs(node.right, sum);
        }
    }
}

class Node {
    Node left, right;
    int val;
    Node(int v) {val = v;}
}```


```python
class Node(object):
    def __init__(self, val):
        self.val = val
        self.left = self.right = None

class Solution(object):
    def pathSum(self, nums):
        self.ans = 0
        root = Node(nums[0] % 10)

        for x in nums[1:]:
            depth, pos, val = x/100, x/10 % 10, x % 10
            pos -= 1
            cur = root
            for d in xrange(depth - 2, -1, -1):
                if pos < 2**d:
                    cur.left = cur = cur.left or Node(val)
                else:
                    cur.right = cur = cur.right or Node(val)

                pos %= 2**d

        def dfs(node, running_sum = 0):
            if not node: return
            running_sum += node.val
            if not node.left and not node.right:
                self.ans += running_sum
            else:
                dfs(node.left, running_sum)
                dfs(node.right, running_sum)

        dfs(root)
        return self.ans```


```java
class Solution {
    int ans = 0;
    Map<Integer, Integer> values;
    public int pathSum(int[] nums) {
        values = new HashMap();
        for (int num: nums)
            values.put(num / 10, num % 10);

        dfs(nums[0] / 10, 0);
        return ans;
    }

    public void dfs(int node, int sum) {
        if (!values.containsKey(node)) return;
        sum += values.get(node);

        int depth = node / 10, pos = node % 10;
        int left = (depth + 1) * 10 + 2 * pos - 1;
        int right = left + 1;

        if (!values.containsKey(left) && !values.containsKey(right)) {
            ans += sum;
        } else {
            dfs(left, sum);
            dfs(right, sum);
        }
    }
}```


```python
class Solution(object):
    def pathSum(self, nums):
        self.ans = 0
        values = {x / 10: x % 10 for x in nums}
        def dfs(node, running_sum = 0):
            if node not in values: return
            running_sum += values[node]
            depth, pos = divmod(node, 10)
            left = (depth + 1) * 10 + 2 * pos - 1
            right = left + 1

            if left not in values and right not in values:
                self.ans += running_sum
            else:
                dfs(left, running_sum)
                dfs(right, running_sum)

        dfs(nums[0] / 10)
        return self.ans```

