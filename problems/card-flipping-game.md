#### Card Flipping Game

```java
class Solution {
    public int flipgame(int[] fronts, int[] backs) {
        Set<Integer> same = new HashSet();
        for (int i = 0; i < fronts.length; ++i)
            if (fronts[i] == backs[i])
                same.add(fronts[i]);

        int ans = 9999;
        for (int x: fronts)
            if (!same.contains(x))
                ans = Math.min(ans, x);

        for (int x: backs)
            if (!same.contains(x))
                ans = Math.min(ans, x);

        return ans % 9999;
    }
}```


```python
class Solution(object):
    def flipgame(self, fronts, backs):
        same = {x for i, x in enumerate(fronts) if x == backs[i]}
        ans = 9999
        for x in itertools.chain(fronts, backs):
            if x not in same:
                ans = min(ans, x)

        return ans % 9999```

