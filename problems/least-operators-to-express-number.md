#### Least Operators to Express Number

```java
class Solution {
    Map<String, Integer> memo;
    int x;
    public int leastOpsExpressTarget(int x, int target) {
        memo = new HashMap();
        this.x = x;
        return dp(0, target) - 1;
    }

    public int dp(int i, int target) {
        String code = "" + i + "#" + target;
        if (memo.containsKey(code))
            return memo.get(code);

        int ans;
        if (target == 0) {
            ans = 0;
        } else if (target == 1) {
            ans = cost(i);
        } else if (i >= 39) {
            ans = target + 1;
        } else {
            int t = target / x;
            int r = target % x;
            ans = Math.min(r * cost(i) + dp(i+1, t),
                           (x-r) * cost(i) + dp(i+1, t+1));
        }

        memo.put(code, ans);
        return ans;
    }

    public int cost(int x) {
        return x > 0 ? x : 2;
    }
}```


```python
from functools import lru_cache

class Solution(object):
    def leastOpsExpressTarget(self, x, target):
        cost = list(range(40))
        cost[0] = 2

        @lru_cache(None)
        def dp(i, targ):
            if targ == 0: return 0
            if targ == 1: return cost[i]
            if i >= 39: return float('inf')

            t, r = divmod(targ, x)
            return min(r * cost[i] + dp(i+1, t),
                       (x-r) * cost[i] + dp(i+1, t+1))

        return dp(0, target) - 1```

