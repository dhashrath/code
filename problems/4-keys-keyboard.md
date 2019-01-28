#### 4 Keys Keyboard

```java
class Solution {
    public int maxA(int N) {
        int[] best = new int[N+1];
        for (int k = 1; k <= N; ++k) {
            best[k] = best[k-1] + 1;
            for (int x = 0; x < k-1; ++x)
                best[k] = Math.max(best[k], best[x] * (k-x-1));
        }
        return best[N];
    }
}```


```python
class Solution(object):
    def maxA(self, N):
        best = [0, 1]
        for k in xrange(2, N+1):
            best.append(max(best[x] * (k-x-1) for x in xrange(k-1)))
            best[-1] = max(best[-1], best[-2] + 1) #addition
        return best[N]```


```java
class Solution {
    public int maxA(int N) {
        int[] best = new int[]{0, 1, 2, 3, 4, 5, 6, 9, 12,
                               16, 20, 27, 36, 48, 64, 81};
        int q = N > 15 ? (N - 11) / 5 : 0;
        return best[N - 5*q] << 2 * q;
    }
}```


```python
class Solution(object):
    def maxA(self, N):
        best = [0, 1, 2, 3, 4, 5, 6, 9, 12,
                16, 20, 27, 36, 48, 64, 81]
        q = (N - 11) / 5 if N > 15 else 0
        return best[N - 5*q] * 4**q```

