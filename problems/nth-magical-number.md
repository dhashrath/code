#### Nth Magical Number

```cpp
class Solution {
public:
    int nthMagicalNumber(int N, int A, int B) {
        int MOD = 1e9 + 7;
        int L = A / gcd(A, B) * B;
        int M = L / A + L / B - 1;
        int q = N / M, r = N % M;

        long ans = (long) q * L % MOD;
        if (r == 0)
            return (int) ans;

        int heads[2] = {A, B};
        for (int i = 0; i < r - 1; ++i) {
            if (heads[0] <= heads[1])
                heads[0] += A;
            else
                heads[1] += B;
        }

        ans += min(heads[0], heads[1]);
        return (int) (ans % MOD);
    }

    int gcd(int x, int y) {
        if (x == 0) return y;
        return gcd(y % x, x);
    }
};```


```java
class Solution {
    public int nthMagicalNumber(int N, int A, int B) {
        int MOD = 1_000_000_007;
        int L = A / gcd(A, B) * B;
        int M = L / A + L / B - 1;
        int q = N / M, r = N % M;

        long ans = (long) q * L % MOD;
        if (r == 0)
            return (int) ans;

        int[] heads = new int[]{A, B};
        for (int i = 0; i < r - 1; ++i) {
            if (heads[0] <= heads[1])
                heads[0] += A;
            else
                heads[1] += B;
        }

        ans += Math.min(heads[0], heads[1]);
        return (int) (ans % MOD);
    }

    public int gcd(int x, int y) {
        if (x == 0) return y;
        return gcd(y % x, x);
    }
}```


```python
class Solution(object):
    def nthMagicalNumber(self, N, A, B):
        from fractions import gcd
        MOD = 10**9 + 7

        L = A / gcd(A, B) * B
        M = L / A + L / B - 1
        q, r = divmod(N, M)

        if r == 0:
            return q * L % MOD

        heads = [A, B]
        for _ in xrange(r - 1):
            if heads[0] <= heads[1]:
                heads[0] += A
            else:
                heads[1] += B

        return (q * L + min(heads)) % MOD```


```javascript
var nthMagicalNumber = function(N, A, B) {
    gcd = (x, y) => {
        if (x == 0) return y;
        return gcd(y % x, x);
    }

    const MOD = 1000000007;
    const L = A / gcd(A, B) * B;
    const M = L / A + L / B - 1;
    const q = Math.trunc(N / M), r = N % M;

    let ans = q * L % MOD;
    if (r == 0)
        return ans;

    let heads = [A, B];
    for (let i = 0; i < r - 1; ++i) {
        if (heads[0] <= heads[1])
            heads[0] += A;
        else
            heads[1] += B;
    }

    ans += Math.min(heads[0], heads[1]);
    return ans % MOD;
};```


```cpp
class Solution {
public:
    int nthMagicalNumber(int N, int A, int B) {
        int MOD = 1e9 + 7;
        int L = A / gcd(A, B) * B;

        long lo = 0;
        long hi = (long) 1e15;
        while (lo < hi) {
            long mi = lo + (hi - lo) / 2;
            // If there are not enough magic numbers below mi...
            if (mi / A + mi / B - mi / L < N)
                lo = mi + 1;
            else
                hi = mi;
        }

        return (int) (lo % MOD);
    }

    int gcd(int x, int y) {
        if (x == 0) return y;
        return gcd(y % x, x);
    }
};```


```java
class Solution {
    public int nthMagicalNumber(int N, int A, int B) {
        int MOD = 1_000_000_007;
        int L = A / gcd(A, B) * B;

        long lo = 0;
        long hi = (long) 1e15;
        while (lo < hi) {
            long mi = lo + (hi - lo) / 2;
            // If there are not enough magic numbers below mi...
            if (mi / A + mi / B - mi / L < N)
                lo = mi + 1;
            else
                hi = mi;
        }

        return (int) (lo % MOD);
    }

    public int gcd(int x, int y) {
        if (x == 0) return y;
        return gcd(y % x, x);
    }
}```


```python
class Solution(object):
    def nthMagicalNumber(self, N, A, B):
        from fractions import gcd
        MOD = 10**9 + 7
        L = A / gcd(A,B) * B

        def magic_below_x(x):
            #How many magical numbers are <= x?
            return x / A + x / B - x / L

        lo = 0
        hi = 10**15
        while lo < hi:
            mi = (lo + hi) / 2
            if magic_below_x(mi) < N:
                lo = mi + 1
            else:
                hi = mi

        return lo % MOD```


```javascript
var nthMagicalNumber = function(N, A, B) {
    gcd = (x, y) => {
        if (x == 0) return y;
        return gcd(y % x, x);
    }

    const MOD = 1000000007;
    const L = A / gcd(A, B) * B;

    let lo = 0;
    let hi = 1e15;
    while (lo < hi) {
        let mi = lo + Math.trunc((hi - lo) / 2);
        // If there are not enough magic numbers below mi...
        if (Math.trunc(mi/A) + Math.trunc(mi/B) - Math.trunc(mi/L) < N)
            lo = mi + 1;
        else
            hi = mi;
    }

    return lo % MOD;
};```

