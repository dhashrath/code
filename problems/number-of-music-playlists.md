#### Number of Music Playlists

```java
class Solution {
    public int numMusicPlaylists(int N, int L, int K) {
        int MOD = 1_000_000_007;

        long[][] dp = new long[L+1][N+1];
        dp[0][0] = 1;
        for (int i = 1; i <= L; ++i)
            for (int j = 1; j <= N; ++j) {
                dp[i][j] += dp[i-1][j-1] * (N-j+1);
                dp[i][j] += dp[i-1][j] * Math.max(j-K, 0);
                dp[i][j] %= MOD;
            }

        return (int) dp[L][N];
    }
}```


```python
from functools import lru_cache

class Solution:
    def numMusicPlaylists(self, N, L, K):
        @lru_cache(None)
        def dp(i, j):
            if i == 0:
                return +(j == 0)
            ans = dp(i-1, j-1) * (N-j+1)
            ans += dp(i-1, j) * max(j-K, 0)
            return ans % (10**9+7)

        return dp(L, N)```


```java
class Solution {
    public int numMusicPlaylists(int N, int L, int K) {
        int MOD = 1_000_000_007;

        // dp[S] at time P = <S, P> as discussed in article
        long[] dp = new long[L-N+1];
        Arrays.fill(dp, 1);
        for (int p = 2; p <= N-K; ++p)
            for (int i = 1; i <= L-N; ++i) {
                dp[i] += dp[i-1] * p;
                dp[i] %= MOD;
            }

        // Multiply by N!
        long ans = dp[L-N];
        for (int k = 2; k <= N; ++k)
            ans = ans * k % MOD;
        return (int) ans;
    }
}```


```python
class Solution(object):
    def numMusicPlaylists(self, N, L, K):
        # dp[S] at time P = <S, P> as discussed in article
        dp = [1] * (L-N+1)
        for p in xrange(2, N-K+1):
            for i in xrange(1, L-N+1):
                dp[i] += dp[i-1] * p

        # Multiply by N!
        ans = dp[-1]
        for k in xrange(2, N+1):
            ans *= k
        return ans % (10**9 + 7)```


```python
class Solution(object):
    def numMusicPlaylists(self, N, L, K):
        MOD = 10**9 + 7
        def inv(x):
            return pow(x, MOD-2, MOD)

        C = 1
        for x in xrange(1, N-K):
            C *= -x
            C %= MOD
        C = inv(C)

        ans = 0
        for k in xrange(1, N-K+1):
            ans += pow(k, L-K-1, MOD) * C
            C = C * (k - (N-K)) % MOD * inv(k) % MOD

        for k in xrange(1, N+1):
            ans = ans * k % MOD
        return ans```

