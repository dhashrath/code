#### Knight Probability in Chessboard

```java
class Solution {
    public double knightProbability(int N, int K, int sr, int sc) {
        double[][] dp = new double[N][N];
        int[] dr = new int[]{2, 2, 1, 1, -1, -1, -2, -2};
        int[] dc = new int[]{1, -1, 2, -2, 2, -2, 1, -1};

        dp[sr][sc] = 1;
        for (; K > 0; K--) {
            double[][] dp2 = new double[N][N];
            for (int r = 0; r < N; r++) {
                for (int c = 0; c < N; c++) {
                    for (int k = 0; k < 8; k++) {
                        int cr = r + dr[k];
                        int cc = c + dc[k];
                        if (0 <= cr && cr < N && 0 <= cc && cc < N) {
                            dp2[cr][cc] += dp[r][c] / 8.0;
                        }
                    }
                }
            }
            dp = dp2;
        }
        double ans = 0.0;
        for (double[] row: dp) {
            for (double x: row) ans += x;
        }
        return ans;
    }
}```


```python
class Solution(object):
    def knightProbability(self, N, K, r, c):
        dp = [[0] * N for _ in xrange(N)]
        dp[r][c] = 1
        for _ in xrange(K):
            dp2 = [[0] * N for _ in xrange(N)]
            for r, row in enumerate(dp):
                for c, val in enumerate(row):
                    for dr, dc in ((2,1),(2,-1),(-2,1),(-2,-1),
                                   (1,2),(1,-2),(-1,2),(-1,-2)):
                        if 0 <= r + dr < N and 0 <= c + dc < N:
                            dp2[r+dr][c+dc] += val / 8.0
            dp = dp2

        return sum(map(sum, dp))

```


```java
class Solution {
    public double knightProbability(int N, int K, int sr, int sc) {
        int[] dr = new int[]{-1, -1, 1, 1, -2, -2, 2, 2};
        int[] dc = new int[]{2, -2, 2, -2, 1, -1, 1, -1};

        int[] index = new int[N * N];
        int t = 0;
        for (int r = 0; r < N; r++) {
            for (int c = 0; c < N; c++) {
                if (r * N + c == canonical(r, c, N)) {
                    index[r * N + c] = t;
                    t++;
                } else {
                    index[r * N + c] = index[canonical(r, c, N)];
                }
            }
        }

        double[][] T = new double[t][t];
        int curRow = 0;
        for (int r = 0; r < N; r++) {
            for (int c = 0; c < N; c++) {
                if (r * N + c == canonical(r, c, N)) {
                    for (int k = 0; k < 8; k++) {
                        int cr = r + dr[k], cc = c + dc[k];
                        if (0 <= cr && cr < N && 0 <= cc && cc < N) {
                            T[curRow][index[canonical(cr, cc, N)]] += 0.125;
                        }
                    }
                    curRow++;
                }
            }
        }

        double[] row = matrixExpo(T, K)[index[sr*N + sc]];
        double ans = 0.0;
        for (double x: row) ans += x;
        return ans;
    }

    public int canonical(int r, int c, int N) {
        if (2*r > N) r = N-1-r;
        if (2*c > N) c = N-1-c;
        if (r > c) {
            int t = r;
            r = c;
            c = t;
        }
        return r * N + c;
    }
    public double[][] matrixMult(double[][] A, double[][] B) {
        double[][] ans = new double[A.length][A.length];
        for (int i = 0; i < A.length; i++) {
            for (int j = 0; j < B[0].length; j++) {
                for (int k = 0; k < B.length; k++) {
                    ans[i][j] += A[i][k] * B[k][j];
                }
            }
        }
        return ans;
    }
    public double[][] matrixExpo(double[][] A, int pow) {
        double[][] ans = new double[A.length][A.length];
        for (int i = 0; i < A.length; i++) ans[i][i] = 1;
        if (pow == 0) return ans;
        if (pow == 1) return A;
        if (pow % 2 == 1) return matrixMult(matrixExpo(A, pow-1), A);
        double[][] B = matrixExpo(A, pow / 2);
        return matrixMult(B, B);
    }
}```


```python
class Solution(object):
    def knightProbability(self, N, K, sr, sc):
        def canonical(r, c):
            if 2 * r > N: r = N - 1 - r
            if 2 * c > N: c = N - 1 - c
            if r > c: r, c = c, r
            return r*N + c

        def matrix_mult(A, B):
            ZB = zip(*B)
            return [[sum(a * b for a, b in zip(row, col))
                     for col in ZB] for row in A]

        def matrix_expo(A, K):
            if K == 0:
                return [[+(i==j) for j in xrange(len(A))]
                        for i in xrange(len(A))]
            if K == 1:
                return A
            elif K % 2:
                return matrix_mult(matrix_expo(A, K-1), A)
            B = matrix_expo(A, K/2)
            return matrix_mult(B, B)

        index = [0] * (N*N)
        t = 0
        for r in xrange(N):
            for c in xrange(N):
                if r*N + c == canonical(r, c):
                    index[r*N + c] = t
                    t += 1
                else:
                    index[r*N + c] = index[canonical(r, c)]

        T = []
        for r in xrange(N):
            for c in xrange(N):
                if r*N + c == canonical(r, c):
                    row = [0] * t
                    for dr, dc in ((2,1),(2,-1),(-2,1),(-2,-1),
                                    (1,2),(1,-2),(-1,2),(-1,-2)):
                        if 0 <= r+dr < N and 0 <= c+dc < N:
                            row[index[(r+dr)*N + c+dc]] += 0.125
                    T.append(row)

        Tk = matrix_expo(T, K)
        i = index[sr * N + sc]
        return sum(Tk[i])```

