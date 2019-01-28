#### Partition Array into Disjoint Intervals

```java
class Solution {
    public int partitionDisjoint(int[] A) {
        int N = A.length;
        int[] maxleft = new int[N];
        int[] minright = new int[N];

        int m = A[0];
        for (int i = 0; i < N; ++i) {
            m = Math.max(m, A[i]);
            maxleft[i] = m;
        }

        m = A[N-1];
        for (int i = N-1; i >= 0; --i) {
            m = Math.min(m, A[i]);
            minright[i] = m;
        }

        for (int i = 1; i < N; ++i)
            if (maxleft[i-1] <= minright[i])
                return i;

        throw null;
    }
}```


```python
class Solution(object):
    def partitionDisjoint(self, A):
        N = len(A)
        maxleft = [None] * N
        minright = [None] * N

        m = A[0]
        for i in xrange(N):
            m = max(m, A[i])
            maxleft[i] = m

        m = A[-1]
        for i in xrange(N-1, -1, -1):
            m = min(m, A[i])
            minright[i] = m

        for i in xrange(1, N):
            if maxleft[i-1] <= minright[i]:
                return i```

