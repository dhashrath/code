#### Tallest Billboard

```java
class Solution {
    int NINF = Integer.MIN_VALUE / 3;
    Integer[][] memo;
    public int tallestBillboard(int[] rods) {
        int N = rods.length;
        // "memo[n][x]" will be stored at memo[n][5000+x]
        // Using Integer for default value null
        memo = new Integer[N][10001];
        return (int) dp(rods, 0, 5000);
    }

    public int dp(int[] rods, int i, int s) {
        if (i == rods.length) {
            return s == 5000 ? 0 : NINF;
        } else if (memo[i][s] != null) {
            return memo[i][s];
        } else {
            int ans = dp(rods, i+1, s);
            ans = Math.max(ans, dp(rods, i+1, s-rods[i]));
            ans = Math.max(ans, rods[i] + dp(rods, i+1, s+rods[i]));
            memo[i][s] = ans;
            return ans;
        }
    }
}```


```python3
from functools import lru_cache
class Solution:
    def tallestBillboard(self, rods):
        @lru_cache(None)
        def dp(i, s):
            if i == len(rods):
                return 0 if s == 0 else float('-inf')
            return max(dp(i + 1, s),
                       dp(i + 1, s - rods[i]),
                       dp(i + 1, s + rods[i]) + rods[i])

        return dp(0, 0)```


```java
import java.awt.Point;

class Solution {
    public int tallestBillboard(int[] rods) {
        int N = rods.length;
        Map<Integer, Integer> Ldelta = make(Arrays.copyOfRange(rods, 0, N/2));
        Map<Integer, Integer> Rdelta = make(Arrays.copyOfRange(rods, N/2, N));

        int ans = 0;
        for (int d: Ldelta.keySet())
            if (Rdelta.containsKey(-d))
                ans = Math.max(ans, Ldelta.get(d) + Rdelta.get(-d));

        return ans;
    }

    public Map<Integer, Integer> make(int[] A) {
        Point[] dp = new Point[60000];
        int t = 0;
        dp[t++] = new Point(0, 0);
        for (int v: A) {
            int stop = t;
            for (int i = 0; i < stop; ++i) {
                Point p = dp[i];
                dp[t++] = new Point(p.x + v, p.y);
                dp[t++] = new Point(p.x, p.y + v);
            }
        }

        Map<Integer, Integer> ans = new HashMap();
        for (int i = 0; i < t; ++i) {
            int a = dp[i].x;
            int b = dp[i].y;
            ans.put(a-b, Math.max(ans.getOrDefault(a-b, 0), a));
        }

        return ans;
    }
}```


```python
class Solution(object):
    def tallestBillboard(self, rods):
        def make(A):
            states = {(0, 0)}
            for x in A:
                states |= ({(a+x, b) for a, b in states} |
                           {(a, b+x) for a, b in states})

            delta = {}
            for a, b in states:
                delta[a-b] = max(delta.get(a-b, 0), a)
            return delta

        N = len(rods)
        Ldelta = make(rods[:N/2])
        Rdelta = make(rods[N/2:])

        ans = 0
        for d in Ldelta:
            if -d in Rdelta:
                ans = max(ans, Ldelta[d] + Rdelta[-d])
        return ans```

