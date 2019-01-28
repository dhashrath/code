#### Kth Smallest Number in Multiplication Table

```java
class Solution {
    public int findKthNumber(int m, int n, int k) {
        int[] table = new int[m*n];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                table[(i - 1) * n + j - 1] = i * j;
            }
        }
        Arrays.sort(table);
        return table[k-1];
    }
}```


```python
class Solution(object):
    def findKthNumber(self, m, n, k):
        table = [i*j for i in range(1, m+1) for j in range(1, n+1)]
        table.sort()
        return table[k-1]```


```java
class Solution {
    public int findKthNumber(int m, int n, int k) {
        PriorityQueue<Node> heap = new PriorityQueue<Node>(m,
            Comparator.<Node> comparingInt(node -> node.val));

        for (int i = 1; i <= m; i++) {
            heap.offer(new Node(i, i));
        }

        Node node = null;
        for (int i = 0; i < k; i++) {
            node = heap.poll();
            int nxt = node.val + node.root;
            if (nxt <= node.root * n) {
                heap.offer(new Node(nxt, node.root));
            }
        }
        return node.val;
    }
}

class Node {
    int val;
    int root;
    public Node(int v, int r) {
        val = v;
        root = r;
    }
}```


```python
class Solution(object):
    def findKthNumber(self, m, n, k):
        heap = [(i, i) for i in range(1, m+1)]
        heapq.heapify(heap)

        for _ in xrange(k):
            val, root = heapq.heappop(heap)
            nxt = val + root
            if nxt <= root * n:
                heapq.heappush(heap, (nxt, root))

        return val```


```java
class Solution {
    public boolean enough(int x, int m, int n, int k) {
        int count = 0;
        for (int i = 1; i <= m; i++) {
            count += Math.min(x / i, n);
        }
        return count >= k;
    }

    public int findKthNumber(int m, int n, int k) {
        int lo = 1, hi = m * n;
        while (lo < hi) {
            int mi = lo + (hi - lo) / 2;
            if (!enough(mi, m, n, k)) lo = mi + 1;
            else hi = mi;
        }
        return lo;
    }
}```


```python
class Solution(object):
    def findKthNumber(self, m, n, k):
        def enough(x):
            count = 0
            for i in xrange(1, m+1):
                count += min(x // i, n)
            return count >= k

        lo, hi = 1, m * n
        while lo < hi:
            mi = (lo + hi) / 2
            if not enough(mi):
                lo = mi + 1
            else:
                hi = mi
        return lo```

