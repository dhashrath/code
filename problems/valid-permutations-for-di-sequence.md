#### Valid Permutations for DI Sequence

```java
class Solution {
    public int numPermsDISequence(String S) {
        int MOD = 1_000_000_007;
        int N = S.length();

        // dp[i][j] : Num ways to place P_i with relative rank j
        int[][] dp = new int[N+1][N+1];
        Arrays.fill(dp[0], 1);

        for (int i = 1; i <= N; ++i) {
            for (int j = 0; j <= i; ++j) {
                if (S.charAt(i-1) == 'D') {
                    for (int k = j; k < i; ++k) {
                        dp[i][j] += dp[i-1][k];
                        dp[i][j] %= MOD;
                    }
                } else {
                    for (int k = 0; k < j; ++k) {
                        dp[i][j] += dp[i-1][k];
                        dp[i][j] %= MOD;
                    }
                }
            }
        }

        int ans = 0;
        for (int x: dp[N]) {
            ans += x;
            ans %= MOD;
        }

        return ans;
    }
}```


```python3
from functools import lru_cache

class Solution:
    def numPermsDISequence(self, S):
        MOD = 10**9 + 7
        N = len(S)

        @lru_cache(None)
        def dp(i, j):
            # How many ways to place P_i with relative rank j?
            if i == 0:
                return 1
            elif S[i-1] == 'D':
                return sum(dp(i-1, k) for k in range(j, i)) % MOD
            else:
                return sum(dp(i-1, k) for k in range(j)) % MOD

        return sum(dp(N, j) for j in range(N+1)) % MOD```


```python3
from functools import lru_cache

class Solution:
    def numPermsDISequence(self, S):
        MOD = 10**9 + 7
        N = len(S)

        @lru_cache(None)
        def dp(i, j):
            # How many ways to place P_i with relative rank j?
            if not(0 <= j <= i):
                return 0
            if i == 0:
                return 1
            elif S[i-1] == 'D':
                return (dp(i, j+1) + dp(i-1, j)) % MOD
            else:
                return (dp(i, j-1) + dp(i-1, j-1)) % MOD

        return sum(dp(N, j) for j in range(N+1)) % MOD```


```python3
from functools import lru_cache

class Solution:
    def numPermsDISequence(self, S):
        MOD = 10**9 + 7

        fac = [1, 1]
        for x in range(2, 201):
            fac.append(fac[-1] * x % MOD)
        facinv = [pow(f, MOD-2, MOD) for f in fac]

        def binom(n, k):
            return fac[n] * facinv[n-k] % MOD * facinv[k] % MOD

        @lru_cache(None)
        def dp(i, j):
            if i >= j: return 1
            ans = 0
            n = j - i + 2
            if S[i] == 'I': ans += dp(i+1, j)
            if S[j] == 'D': ans += dp(i, j-1)

            for k in range(i+1, j+1):
                if S[k-1:k+1] == 'DI':
                    ans += binom(n-1, k-i) * dp(i, k-2) % MOD * dp(k+1, j) % MOD
                    ans %= MOD
            return ans

        return dp(0, len(S) - 1)```

