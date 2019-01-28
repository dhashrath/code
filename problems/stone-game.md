#### Stone Game

```cpp
class Solution {
public:
    bool stoneGame(vector<int>& piles) {
        int N = piles.size();

        // dp[i+1][j+1] = the value of the game [piles[i], ..., piles[j]]
        int dp[N+2][N+2];
        memset(dp, 0, sizeof(dp));

        for (int size = 1; size <= N; ++size)
            for (int i = 0, j = size - 1; j < N; ++i, ++j) {
                int parity = (j + i + N) % 2;  // j - i - N; but +x = -x (mod 2)
                if (parity == 1)
                    dp[i+1][j+1] = max(piles[i] + dp[i+2][j+1], piles[j] + dp[i+1][j]);
                else
                    dp[i+1][j+1] = min(-piles[i] + dp[i+2][j+1], -piles[j] + dp[i+1][j]);
            }

        return dp[1][N] > 0;
    }
};```


```java
class Solution {
    public boolean stoneGame(int[] piles) {
        int N = piles.length;

        // dp[i+1][j+1] = the value of the game [piles[i], ..., piles[j]].
        int[][] dp = new int[N+2][N+2];
        for (int size = 1; size <= N; ++size)
            for (int i = 0; i + size <= N; ++i) {
                int j = i + size - 1;
                int parity = (j + i + N) % 2;  // j - i - N; but +x = -x (mod 2)
                if (parity == 1)
                    dp[i+1][j+1] = Math.max(piles[i] + dp[i+2][j+1], piles[j] + dp[i+1][j]);
                else
                    dp[i+1][j+1] = Math.min(-piles[i] + dp[i+2][j+1], -piles[j] + dp[i+1][j]);
            }

        return dp[1][N] > 0;
    }
}```


```javascript
var stoneGame = function(piles) {
    const N = piles.length;
    // Make a (N+2) by (N+2) array, initialized with 0s.
    const dp = Array(N + 2).fill(0).map(() => Array(N + 2).fill(0));

    // dp[i+1][j+1] = the value of the game [piles[i], ..., piles[j]]
    for (let size = 1; size <= N; ++size)
        for (let i = 0, j = size - 1; j < N; ++i, ++j) {
            let parity = (j + i + N) % 2;  // j - i - N; but +x = -x (mod 2)
            if (parity == 1)
                dp[i+1][j+1] = Math.max(piles[i] + dp[i+2][j+1], piles[j] + dp[i+1][j]);
            else
                dp[i+1][j+1] = Math.min(-piles[i] + dp[i+2][j+1], -piles[j] + dp[i+1][j]);
        }

    return dp[1][N] > 0;
};```


```python3
from functools import lru_cache

class Solution:
    def stoneGame(self, piles):
        N = len(piles)

        @lru_cache(None)
        def dp(i, j):
            # The value of the game [piles[i], piles[i+1], ..., piles[j]].
            if i > j: return 0
            parity = (j - i - N) % 2
            if parity == 1:  # first player
                return max(piles[i] + dp(i+1,j), piles[j] + dp(i,j-1))
            else:
                return min(-piles[i] + dp(i+1,j), -piles[j] + dp(i,j-1))

        return dp(0, N - 1) > 0```


```cpp
class Solution {
public:
    bool stoneGame(vector<int>& piles) {
        return true;
    }
};```


```java
class Solution {
    public boolean stoneGame(int[] piles) {
        return true;
    }
}```


```python
class Solution:
    def stoneGame(self, piles):
        return True```


```javascript
var stoneGame = function(piles) {
    return true;
};```

