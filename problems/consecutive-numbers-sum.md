#### Consecutive Numbers Sum

```java
class Solution {
    public int consecutiveNumbersSum(int N) {
        int ans = 0;
        for (int start = 1; start <= N; ++start) {
            int target = N, x = start;
            while (target > 0)
                target -= x++;
            if (target == 0) ans++;
        }
        return ans;
    }
}```


```python
class Solution(object):
    def consecutiveNumbersSum(self, N):
        ans = 0
        for start in xrange(1, N+1):
            target = N
            while target > 0:
                target -= start
                start += 1
            if target == 0: ans += 1
        return ans```


```java
class Solution {
    public int consecutiveNumbersSum(int N) {
        // 2N = k(2x + k + 1)
        int ans = 0;
        for (int k = 1; k <= 2*N; ++k)
            if (2 * N % k == 0) {
                int y = 2 * N / k - k - 1;
                if (y % 2 == 0 && y >= 0)
                    ans++;
            }
        return ans;
    }
}```


```python
class Solution(object):
    def consecutiveNumbersSum(self, N):
        # 2N = k(2x + k + 1)
        ans = 0
        for k in xrange(1, 2*N + 1):
            if 2*N % k == 0:
                y = 2 * N / k - k - 1
                if y % 2 == 0 and y >= 0:
                    ans += 1
        return ans```


```java
class Solution {
    public int consecutiveNumbersSum(int N) {
        while ((N & 1) == 0) N >>= 1;
        int ans = 1, d = 3;

        while (d * d <= N) {
            int e = 0;
            while (N % d == 0) {
                N /= d;
                e++;
            }
            ans *= e + 1;
            d += 2;
        }

        if (N > 1) ans <<= 1;
        return ans;
    }
}```


```python
class Solution(object):
    def consecutiveNumbersSum(self, N):
        while N & 1 == 0:
            N >>= 1

        ans = 1    
        d = 3
        while d * d <= N:
            e = 0
            while N % d == 0:
                N /= d
                e += 1
            ans *= e + 1
            d += 2

        if N > 1: ans *= 2
        return ans```

