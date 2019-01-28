#### Soup Servings

```java
class Solution {
    public double soupServings(int N) {
        N = N/25 + (N%25 > 0 ? 1 : 0);
        if (N >= 500) return 1.0;

        double[][] memo = new double[N+1][N+1];
        for (int s = 0; s <= 2*N; ++s) {
            for (int i = 0; i <= N; ++i) {
                int j = s-i;
                if (j < 0 || j > N) continue;
                double ans = 0.0;
                if (i == 0) ans = 1.0;
                if (i == 0 && j == 0) ans = 0.5;
                if (i > 0 && j > 0) {
                    ans = 0.25 * (memo[M(i-4)][j] + memo[M(i-3)][M(j-1)] +
                                  memo[M(i-2)][M(j-2)] + memo[M(i-1)][M(j-3)]);
                }
                memo[i][j] = ans;

            }
        }
        return memo[N][N];
    }

    public int M(int x) { return Math.max(0, x); }
}```


```python
class Solution(object):
    def soupServings(self, N):
        Q, R = divmod(N, 25)
        N = Q + (R > 0)
        if N >= 500: return 1

        memo = {}
        def dp(x, y):
            if (x, y) not in memo:
                if x <= 0 or y <= 0:
                    ans = 0.5 if x<=0 and y<=0 else 1.0 if x<=0 else 0.0
                else:
                    ans = 0.25 * (dp(x-4,y)+dp(x-3,y-1)+dp(x-2,y-2)+dp(x-1,y-3))
                memo[x, y] = ans
            return memo[x, y]

        return dp(N, N)```

