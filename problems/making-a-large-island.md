#### Making A Large Island

```java
class Solution {
    int[] dr = new int[]{-1, 0, 1, 0};
    int[] dc = new int[]{0, -1, 0, 1};

    public int largestIsland(int[][] grid) {
        int N = grid.length;

        int ans = 0;
        boolean hasZero = false;
        for (int r = 0; r < N; ++r)
            for (int c = 0; c < N; ++c)
                if (grid[r][c] == 0) {
                    hasZero = true;
                    grid[r][c] = 1;
                    ans = Math.max(ans, check(grid, r, c));
                    grid[r][c] = 0;
                }

        return hasZero ? ans : N*N;
    }

    public int check(int[][] grid, int r0, int c0) {
        int N = grid.length;
        Stack<Integer> stack = new Stack();
        Set<Integer> seen = new HashSet();
        stack.push(r0 * N + c0);
        seen.add(r0 * N + c0);

        while (!stack.isEmpty()) {
            int code = stack.pop();
            int r = code / N, c = code % N;
            for (int k = 0; k < 4; ++k) {
                int nr = r + dr[k], nc = c + dc[k];
                if (!seen.contains(nr * N + nc) && 0 <= nr && nr < N &&
                        0 <= nc && nc < N && grid[nr][nc] == 1) {
                    stack.push(nr * N + nc);
                    seen.add(nr * N + nc);
                }
            }
        }

        return seen.size();
    }
}
```


```python
class Solution(object):
    def largestIsland(self, grid):
        N = len(grid)

        def check(r, c):
            seen = {(r, c)}
            stack = [(r, c)]
            while stack:
                r, c = stack.pop()
                for nr, nc in ((r-1, c), (r, c-1), (r+1, c), (r, c+1)):
                    if (nr, nc) not in seen and 0 <= nr < N and 0 <= nc < N and grid[nr][nc]:
                        stack.append((nr, nc))
                        seen.add((nr, nc))
            return len(seen)

        ans = 0
        has_zero = False
        for r, row in enumerate(grid):
            for c, val in enumerate(row):
                if val == 0:
                    has_zero = True
                    grid[r][c] = 1
                    ans = max(ans, check(r, c))
                    grid[r][c] = 0

        return ans if has_zero else N*N```


```java
class Solution {
    int[] dr = new int[]{-1, 0, 1, 0};
    int[] dc = new int[]{0, -1, 0, 1};
    int[][] grid;
    int N;

    public int largestIsland(int[][] grid) {
        this.grid = grid;
        N = grid.length;

        int index = 2;
        int[] area = new int[N*N + 2];
        for (int r = 0; r < N; ++r)
            for (int c = 0; c < N; ++c)
                if (grid[r][c] == 1)
                    area[index] = dfs(r, c, index++);

        int ans = 0;
        for (int x: area) ans = Math.max(ans, x);
        for (int r = 0; r < N; ++r)
            for (int c = 0; c < N; ++c)
                if (grid[r][c] == 0) {
                    Set<Integer> seen = new HashSet();
                    for (Integer move: neighbors(r, c))
                        if (grid[move / N][move % N] > 1)
                            seen.add(grid[move / N][move % N]);

                    int bns = 1;
                    for (int i: seen) bns += area[i];
                    ans = Math.max(ans, bns);
                }

        return ans;
    }

    public int dfs(int r, int c, int index) {
        int ans = 1;
        grid[r][c] = index;
        for (Integer move: neighbors(r, c)) {
            if (grid[move / N][move % N] == 1) {
                grid[move / N][move % N] = index;
                ans += dfs(move / N, move % N, index);
            }
        }

        return ans;
    }

    public List<Integer> neighbors(int r, int c) {
        List<Integer> ans = new ArrayList();
        for (int k = 0; k < 4; ++k) {
            int nr = r + dr[k];
            int nc = c + dc[k];
            if (0 <= nr && nr < N && 0 <= nc && nc < N)
                ans.add(nr * N + nc);
        }

        return ans;
    }
}```


```python
class Solution(object):
    def largestIsland(self, grid):
        N = len(grid)

        def neighbors(r, c):
            for nr, nc in ((r-1, c), (r+1, c), (r, c-1), (r, c+1)):
                if 0 <= nr < N and 0 <= nc < N:
                    yield nr, nc

        def dfs(r, c, index):
            ans = 1
            grid[r][c] = index
            for nr, nc in neighbors(r, c):
                if grid[nr][nc] == 1:
                    ans += dfs(nr, nc, index)
            return ans

        area = {}
        index = 2
        for r in xrange(N):
            for c in xrange(N):
                if grid[r][c] == 1:
                    area[index] = dfs(r, c, index)
                    index += 1

        ans = max(area.values() or [0])
        for r in xrange(N):
            for c in xrange(N):
                if grid[r][c] == 0:
                    seen = {grid[nr][nc] for nr, nc in neighbors(r, c) if grid[nr][nc] > 1}
                    ans = max(ans, 1 + sum(area[i] for i in seen))
        return ans```

