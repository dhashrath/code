#### Smallest Rotation with Highest Score

```java
class Solution {
    public int bestRotation(int[] A) {
        int N = A.length;
        int[] bad = new int[N];
        for (int i = 0; i < N; ++i) {
            int left = (i - A[i] + 1 + N) % N;
            int right = (i + 1) % N;
            bad[left]--;
            bad[right]++;
            if (left > right)
                bad[0]--;
        }

        int best = -N;
        int ans = 0, cur = 0;
        for (int i = 0; i < N; ++i) {
            cur += bad[i];
            if (cur > best) {
                best = cur;
                ans = i;
            }
        }
        return ans;
    }
}```


```python
class Solution(object):
    def bestRotation(self, A):
        N = len(A)
        bad = [0] * N
        for i, x in enumerate(A):
            left, right = (i - x + 1) % N, (i + 1) % N
            bad[left] -= 1
            bad[right] += 1
            if left > right:
                bad[0] -= 1

        best = -N
        ans = cur = 0
        for i, score in enumerate(bad):
            cur += score
            if cur > best:
                best = cur
                ans = i

        return ans```

