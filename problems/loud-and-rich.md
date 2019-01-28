#### Loud and Rich

```java
class Solution {
    ArrayList<Integer>[] graph;
    int[] answer;
    int[] quiet;

    public int[] loudAndRich(int[][] richer, int[] quiet) {
        int N = quiet.length;
        graph = new ArrayList[N];
        answer = new int[N];
        this.quiet = quiet;

        for (int node = 0; node < N; ++node)
            graph[node] = new ArrayList<Integer>();

        for (int[] edge: richer)
            graph[edge[1]].add(edge[0]);

        Arrays.fill(answer, -1);

        for (int node = 0; node < N; ++node)
            dfs(node);
        return answer;
    }

    public int dfs(int node) {
        if (answer[node] == -1) {
            answer[node] = node;
            for (int child: graph[node]) {
                int cand = dfs(child);
                if (quiet[cand] < quiet[answer[node]])
                    answer[node] = cand;
            }
        }
        return answer[node];
    }
}```


```python
class Solution(object):
    def loudAndRich(self, richer, quiet):
        N = len(quiet)
        graph = [[] for _ in xrange(N)]
        for u, v in richer:
            graph[v].append(u)

        answer = [None] * N
        def dfs(node):
            #Want least quiet person in this subtree
            if answer[node] is None:
                answer[node] = node
                for child in graph[node]:
                    cand = dfs(child)
                    if quiet[cand] < quiet[answer[node]]:
                        answer[node] = cand
            return answer[node]

        return map(dfs, range(N))```

