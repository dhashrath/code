#### Shortest Path Visiting All Nodes

```java
class Solution {
    public int shortestPathLength(int[][] graph) {
        int N = graph.length;
        Queue<State> queue = new LinkedList();
        int[][] dist = new int[1<<N][N];
        for (int[] row: dist) Arrays.fill(row, N*N);

        for (int x = 0; x < N; ++x) {
            queue.offer(new State(1<<x, x));
            dist[1 << x][x] = 0;
        }

        while (!queue.isEmpty()) {
            State node = queue.poll();
            int d = dist[node.cover][node.head];
            if (node.cover == (1<<N) - 1) return d;

            for (int child: graph[node.head]) {
                int cover2 = node.cover | (1 << child);
                if (d + 1 < dist[cover2][child]) {
                    dist[cover2][child] = d + 1;
                    queue.offer(new State(cover2, child));

                }
            }
        }

        throw null;
    }
}

class State {
    int cover, head;
    State(int c, int h) {
        cover = c;
        head = h;
    }
}```


```python
class Solution(object):
    def shortestPathLength(self, graph):
        N = len(graph)
        queue = collections.deque((1 << x, x) for x in xrange(N))
        dist = collections.defaultdict(lambda: N*N)
        for x in xrange(N): dist[1 << x, x] = 0

        while queue:
            cover, head = queue.popleft()
            d = dist[cover, head]
            if cover == 2**N - 1: return d
            for child in graph[head]:
                cover2 = cover | (1 << child)
                if d + 1 < dist[cover2, child]:
                    dist[cover2, child] = d + 1
                    queue.append((cover2, child))```


```java
class Solution {
    public int shortestPathLength(int[][] graph) {
        int N = graph.length;
        int dist[][] = new int[1 << N][N];
        for (int[] row: dist) Arrays.fill(row, N*N);
        for (int x = 0; x < N; ++x) dist[1<<x][x] = 0;

        for (int cover = 0; cover < 1 << N; ++cover) {
            boolean repeat = true;
            while (repeat) {
                repeat = false;
                for (int head = 0; head < N; ++head) {
                    int d = dist[cover][head];
                    for (int next: graph[head]) {
                        int cover2 = cover | (1 << next);
                        if (d + 1 < dist[cover2][next]) {
                            dist[cover2][next] = d+1;
                            if (cover == cover2) repeat = true;
                        }
                    }
                }
            }
        }

        int ans = N*N;
        for (int cand: dist[(1<<N) - 1])
            ans = Math.min(cand, ans);
        return ans;
    }
}```


```python
class Solution(object):
    def shortestPathLength(self, graph):
        N = len(graph)
        dist = [[float('inf')] * N for i in xrange(1 << N)]
        for x in xrange(N):
            dist[1<<x][x] = 0

        for cover in xrange(1 << N):
            repeat = True
            while repeat:
                repeat = False
                for head, d in enumerate(dist[cover]):
                    for nei in graph[head]:
                        cover2 = cover | (1 << nei)
                        if d + 1 < dist[cover2][nei]:
                            dist[cover2][nei] = d + 1
                            if cover == cover2:
                                repeat = True

        return min(dist[2**N - 1])```

