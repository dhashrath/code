#### Maximum Width Ramp

```java
class Solution {
    public int maxWidthRamp(int[] A) {
        int N = A.length;
        Integer[] B = new Integer[N];
        for (int i = 0; i < N; ++i)
            B[i] = i;

        Arrays.sort(B, (i, j) -> ((Integer) A[i]).compareTo(A[j]));

        int ans = 0;
        int m = N;
        for (int i: B) {
            ans = Math.max(ans, i - m);
            m = Math.min(m, i);
        }

        return ans;
    }
}```


```python
class Solution(object):
    def maxWidthRamp(self, A):
        ans = 0
        m = float('inf')
        for i in sorted(range(len(A)), key = A.__getitem__):
            ans = max(ans, i - m)
            m = min(m, i)
        return ans```


```java
import java.awt.Point;

class Solution {
    public int maxWidthRamp(int[] A) {
        int N = A.length;

        int ans = 0;
        List<Point> candidates = new ArrayList();
        candidates.add(new Point(A[N-1], N-1));

        // candidates: i's decreasing, by increasing value of A[i]
        for (int i = N-2; i >= 0; --i) {
            // Find largest j in candidates with A[j] >= A[i]
            int lo = 0, hi = candidates.size();
            while (lo < hi) {
                int mi = lo + (hi - lo) / 2;
                if (candidates.get(mi).x < A[i])
                    lo = mi + 1;
                else
                    hi = mi;
            }

            if (lo < candidates.size()) {
                int j = candidates.get(lo).y;
                ans = Math.max(ans, j - i);
            } else {
                candidates.add(new Point(A[i], i));
            }
        }
        return ans;
    }
}```


```python
class Solution(object):
    def maxWidthRamp(self, A):
        N = len(A)

        ans = 0
        candidates = [(A[N-1], N-1)]
        # candidates: i's decreasing, by increasing value of A[i]
        for i in xrange(N-2, -1, -1):
            # Find largest j in candidates with A[j] >= A[i]
            jx = bisect.bisect(candidates, (A[i],))
            if jx < len(candidates):
                ans = max(ans, candidates[jx][1] - i)
            else:
                candidates.append((A[i], i))

        return ans```

