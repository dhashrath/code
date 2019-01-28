#### Similar String Groups

```java
class Solution {
    public int numSimilarGroups(String[] A) {
        int N = A.length;
        int W = A[0].length();
        DSU dsu = new DSU(N);

        if (N < W*W) { // If few words, then check for pairwise similarity: O(N^2 W)
            for (int i = 0; i < N; ++i)
                for (int j = i+1; j < N; ++j)
                    if (similar(A[i], A[j]))
                        dsu.union(i, j);

        } else { // If short words, check all neighbors: O(N W^3)
            Map<String, List<Integer>> buckets = new HashMap();
            for (int i = 0; i < N; ++i) {
                char[] L = A[i].toCharArray();
                for (int j0 = 0; j0 < L.length; ++j0)
                    for (int j1 = j0 + 1; j1 < L.length; ++j1) {
                        swap(L, j0, j1);
                        StringBuilder sb = new StringBuilder();
                        for (char c: L) sb.append(c);
                        buckets.computeIfAbsent(sb.toString(),
                                x-> new ArrayList<Integer>()).add(i);
                        swap(L, j0, j1);
                    }
            }

            for (int i1 = 0; i1 < A.length; ++i1)
                if (buckets.containsKey(A[i1]))
                    for (int i2: buckets.get(A[i1]))
                        dsu.union(i1, i2);
        }

        int ans = 0;
        for (int i = 0; i < N; ++i)
            if (dsu.parent[i] == i) ans++;

        return ans;
    }

    public boolean similar(String word1, String word2) {
        int diff = 0;
        for (int i = 0; i < word1.length(); ++i)
            if (word1.charAt(i) != word2.charAt(i))
                diff++;
        return diff <= 2;
    }

    public void swap(char[] A, int i, int j) {
        char tmp = A[i];
        A[i] = A[j];
        A[j] = tmp;
    }
}

class DSU {
    int[] parent;
    public DSU(int N) {
        parent = new int[N];
        for (int i = 0; i < N; ++i)
            parent[i] = i;
    }
    public int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }
    public void union(int x, int y) {
        parent[find(x)] = find(y);
    }
}```


```python
class DSU:
    def __init__(self, N):
        self.par = range(N)
    def find(self, x):
        if self.par[x] != x:
            self.par[x] = self.find(self.par[x])
        return self.par[x]
    def union(self, x, y):
        self.par[self.find(x)] = self.find(y)

class Solution(object): # (NW) * min(N, W*W) complexity
    def numSimilarGroups(self, A):
        N, W = len(A), len(A[0])

        def similar(word1, word2):
            diff = 0
            for x, y in itertools.izip(word1, word2):
                if x != y:
                    diff += 1
            return diff <= 2

        dsu = DSU(N)

        if N < W*W: # If few words, then check for pairwise similarity: O(N^2 W)
            for (i1, word1), (i2, word2) in \
                    itertools.combinations(enumerate(A), 2):
                if similar(word1, word2):
                    dsu.union(i1, i2)

        else: # If short words, check all neighbors: O(N W^3)
            buckets = collections.defaultdict(set)
            for i, word in enumerate(A):
                L = list(word)
                for j0, j1 in itertools.combinations(xrange(W), 2):
                    L[j0], L[j1] = L[j1], L[j0]
                    buckets["".join(L)].add(i)
                    L[j0], L[j1] = L[j1], L[j0]

            for i1, word in enumerate(A):
                for i2 in buckets[word]:
                    dsu.union(i1, i2)

        return sum(dsu.par[x] == x for x in xrange(N))```

