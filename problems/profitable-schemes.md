#### Profitable Schemes

```cpp
class Solution {
public:
    int profitableSchemes(int G, int P, vector<int>& group, vector<int>& profit) {
        int MOD = 1e9 + 7;
        int N = group.size();

        long dp[2][P+1][G+1];
        memset(dp, 0, sizeof(dp));
        dp[0][0][0] = 1;

        for (int i = 0; i < N; ++i) {
            int p0 = profit[i];  // the current crime profit
            int g0 = group[i];  // the current crime group size

            long (*cur)[G+1] = dp[i % 2];
            long (*cur2)[G+1] = dp[(i + 1) % 2];

            // Deep copy cur into cur2
            for (int jp = 0; jp <= P; ++jp)
                for (int jg = 0; jg <= G; ++jg)
                    cur2[jp][jg] = cur[jp][jg];

            for (int p1 = 0; p1 <= P; ++p1) {  // p1 : the current profit
                // p2 : the new profit after committing this crime
                int p2 = min(p1 + p0, P);
                for (int g1 = 0; g1 <= G - g0; ++g1) {  // g1 : the current group size
                    // g2 : the new group size after committing this crime
                    int g2 = g1 + g0;
                    cur2[p2][g2] += cur[p1][g1];
                    cur2[p2][g2] %= MOD;
                }
            }
        }

        // Sum all schemes with profit P and group size 0 <= g <= G.
        long ans = 0;
        for (long x: dp[N % 2][P])
            ans += x;

        return (int) (ans % MOD);
    }
};```


```java
class Solution {
    public int profitableSchemes(int G, int P, int[] group, int[] profit) {
        int MOD = 1_000_000_007;
        int N = group.length;
        long[][][] dp = new long[2][P+1][G+1];
        dp[0][0][0] = 1;

        for (int i = 0; i < N; ++i) {
            int p0 = profit[i];  // the current crime profit
            int g0 = group[i];  // the current crime group size

            long[][] cur = dp[i % 2];
            long[][] cur2 = dp[(i + 1) % 2];

            // Deep copy cur into cur2
            for (int jp = 0; jp <= P; ++jp)
                for (int jg = 0; jg <= G; ++jg)
                    cur2[jp][jg] = cur[jp][jg];

            for (int p1 = 0; p1 <= P; ++p1) {  // p1 : the current profit
                // p2 : the new profit after committing this crime
                int p2 = Math.min(p1 + p0, P);
                for (int g1 = 0; g1 <= G - g0; ++g1) {  // g1 : the current group size
                    // g2 : the new group size after committing this crime
                    int g2 = g1 + g0;
                    cur2[p2][g2] += cur[p1][g1];
                    cur2[p2][g2] %= MOD;
                }
            }
        }

        // Sum all schemes with profit P and group size 0 <= g <= G.
        long ans = 0;
        for (long x: dp[N%2][P])
            ans += x;

        return (int) ans;
    }
}```


```python
class Solution(object):
    def profitableSchemes(self, G, P, group, profit):
        MOD = 10**9 + 7
        cur = [[0] * (G + 1) for _ in xrange(P + 1)]
        cur[0][0] = 1

        for p0, g0 in zip(profit, group):
            # p0, g0 : the current crime profit and group size
            cur2 = [row[:] for row in cur]
            for p1 in xrange(P + 1):
                # p1 : the current profit
                # p2 : the new profit after committing this crime
                p2 = min(p1 + p0, P)
                for g1 in xrange(G - g0 + 1):
                    # g1 : the current group size
                    # g2 : the new group size after committing this crime
                    g2 = g1 + g0
                    cur2[p2][g2] += cur[p1][g1]
                    cur2[p2][g2] %= MOD
            cur = cur2

        # Sum all schemes with profit P and group size 0 <= g <= G.
        return sum(cur[-1]) % MOD```


```javascript
var profitableSchemes = function(G, P, group, profit) {
    const MOD = 1e9 + 7;
    const N = group.length;
    let cur = Array(P + 1).fill(0).map(() => Array(G + 1).fill(0));
    cur[0][0] = 1;

    for (let i = 0; i < N; ++i) {
        // p0, g0 : the current crime profit and group size
        let p0 = profit[i];
        let g0 = group[i];

        // Set cur2 as a deep copy of cur.
        let cur2 = Array(P + 1).fill(0).map((_, i) => cur[i].slice(0));

        for (let p1 = 0; p1 <= P; ++p1) {  // p1 : the current profit
            // p2 : the new profit after committing this crime
            let p2 = Math.min(p1 + p0, P);
            for (let g1 = 0; g1 <= G - g0; ++g1) {  // g1 : the current group size
                // g2 : the new group size after committing this crime
                let g2 = g1 + g0;
                cur2[p2][g2] += cur[p1][g1];
                cur2[p2][g2] %= MOD;
            }
        }

        cur = cur2;
    }

    // Sum all schemes with profit P and group size 0 <= g <= G.
    ans = 0;
    for (let x of cur[P])
        ans += x;
    return ans % MOD;
};```

