#### Powerful Integers

```java
class Solution {
    public List<Integer> powerfulIntegers(int x, int y, int bound) {
        Set<Integer> seen = new HashSet();
        for (int i = 0; i < 18 && Math.pow(x, i) <= bound; ++i)
            for (int j = 0; j < 18 && Math.pow(y, j) <= bound; ++j) {
                int v = (int) Math.pow(x, i) + (int) Math.pow(y, j);
                if (v <= bound)
                    seen.add(v);
            }

        return new ArrayList(seen);
    }
}```


```python
class Solution(object): 
    def powerfulIntegers(self, x, y, bound):
        ans = set()
        # 2**18 > bound
        for i in xrange(18):
            for j in xrange(18):
                v = x**i + y**j
                if v <= bound:
                    ans.add(v)
        return list(ans)```

