#### Score After Flipping Matrix

```java
class Solution {
    public int matrixScore(int[][] A) {
        int R = A.length, C = A[0].length;
        int[] colsums = new int[C];
        for (int r = 0; r < R; ++r)
            for (int c = 0; c < C; ++c)
                colsums[c] += A[r][c];

        int ans = 0;
        for (int state = 0; state < (1<<R); ++state) {
            // Toggle the rows so that after, 'state' represents
            // the toggled rows.
            if (state > 0) {
                int trans = state ^ (state-1);
                for (int r = 0; r < R; ++r) {
                    if (((trans >> r) & 1) > 0) {
                        for (int c = 0; c < C; ++c) {
                            colsums[c] += A[r][c] == 1 ? -1 : 1;
                            A[r][c] ^= 1;
                        }
                    }
                }
            }

            // Calculate the score with the rows toggled by 'state'
            int score = 0;
            for (int c = 0; c < C; ++c)
                score += Math.max(colsums[c], R - colsums[c]) * (1 << (C-1-c));
            ans = Math.max(ans, score);
        }

        return ans;
    }
}```


```python
class Solution(object):
    def matrixScore(self, A):
        R, C = len(A), len(A[0])

        colsums = [0] * C
        for r in xrange(R):
            for c in xrange(C):
                colsums[c] += A[r][c]

        ans = 0
        for r in xrange(1<<R):
            if r:
                trans = r ^ (r-1)
                for bit in xrange(R):
                    if (trans >> bit) & 1:
                        for c in xrange(C):
                            colsums[c] += -1 if A[bit][c] else +1
                            A[bit][c] ^= 1
            
            score = sum(max(x, R - x) * (1 << (C-1-c))
                        for c, x in enumerate(colsums))
            ans = max(ans, score)

        return ans```


```java
class Solution {
    public int matrixScore(int[][] A) {
        int R = A.length, C = A[0].length;
        int ans = 0;
        for (int c = 0; c < C; ++c) {
            int col = 0;
            for (int r = 0; r < R; ++r)
                col += A[r][c] ^ A[r][0];
            ans += Math.max(col, R - col) * (1 << (C-1-c));
        }
        return ans;
    }
}```


```python
class Solution(object):
    def matrixScore(self, A):
        R, C = len(A), len(A[0])
        ans = 0
        for c in xrange(C):
            col = 0
            for r in xrange(R):
                col += A[r][c] ^ A[r][0]
            ans += max(col, R - col) * 2 ** (C - 1 - c)
        return ans```

