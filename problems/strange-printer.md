#### Strange Printer

```java
class Solution {
    int[][] memo;
    public int strangePrinter(String s) {
        int N = s.length();
        memo = new int[N][N];
        return dp(s, 0, N - 1);
    }
    public int dp(String s, int i, int j) {
        if (i > j) return 0;
        if (memo[i][j] == 0) {
            int ans = dp(s, i+1, j) + 1;
            for (int k = i+1; k <= j; ++k)
                if (s.charAt(k) == s.charAt(i))
                    ans = Math.min(ans, dp(s, i, k-1) + dp(s, k+1, j));
            memo[i][j] = ans;
        }
        return memo[i][j];
    }
}```


```python
def strangePrinter(self, S):
    memo = {}
    def dp(i, j):
        if i > j: return 0
        if (i, j) not in memo:
            ans = dp(i+1, j) + 1
            for k in xrange(i+1, j+1):
                if S[k] == S[i]:
                    ans = min(ans, dp(i, k-1) + dp(k+1, j))
            memo[i, j] = ans
        return memo[i, j]

    return dp(0, len(S) - 1)```

