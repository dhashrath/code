#### Image Overlap

```java
import java.awt.Point;

class Solution {
    public int largestOverlap(int[][] A, int[][] B) {
        int N = A.length;
        List<Point> A2 = new ArrayList(), B2 = new ArrayList();
        for (int i = 0; i < N*N; ++i) {
            if (A[i/N][i%N] == 1) A2.add(new Point(i/N, i%N));
            if (B[i/N][i%N] == 1) B2.add(new Point(i/N, i%N));
        }

        Set<Point> Bset = new HashSet(B2);

        int ans = 0;
        Set<Point> seen = new HashSet();
        for (Point a: A2) for (Point b: B2) {
            Point delta = new Point(b.x - a.x, b.y - a.y);
            if (!seen.contains(delta)) {
                seen.add(delta);
                int cand = 0;
                for (Point p: A2)
                    if (Bset.contains(new Point(p.x + delta.x, p.y + delta.y)))
                        cand++;
                ans = Math.max(ans, cand);
            }
        }

        return ans;
    }
}```


```python
class Solution(object):
    def largestOverlap(self, A, B):
        N = len(A)
        A2 = [complex(r, c) for r, row in enumerate(A)
              for c, v in enumerate(row) if v]
        B2 = [complex(r, c) for r, row in enumerate(B)
              for c, v in enumerate(row) if v]
        Bset = set(B2)
        seen = set()
        ans = 0
        for a in A2:
            for b in B2:
                d = b-a
                if d not in seen:
                    seen.add(d)
                    ans = max(ans, sum(x+d in Bset for x in A2))
        return ans```


```java
class Solution {
    public int largestOverlap(int[][] A, int[][] B) {
        int N = A.length;
        int[][] count = new int[2*N+1][2*N+1];
        for (int i = 0; i < N; ++i)
            for (int j = 0; j < N; ++j)
                if (A[i][j] == 1)
                    for (int i2 = 0; i2 < N; ++i2)
                        for (int j2 = 0; j2 < N; ++j2)
                            if (B[i2][j2] == 1)
                                count[i-i2 +N][j-j2 +N] += 1;

        int ans = 0;
        for (int[] row: count)
            for (int v: row)
                ans = Math.max(ans, v);
        return ans;
    }
}```


```python
class Solution(object):
    def largestOverlap(self, A, B):
        N = len(A)
        count = collections.Counter()
        for i, row in enumerate(A):
            for j, v in enumerate(row):
                if v:
                    for i2, row2 in enumerate(B):
                        for j2, v2 in enumerate(row2):
                            if v2:
                                count[i-i2, j-j2] += 1
        return max(count.values() or [0])```

