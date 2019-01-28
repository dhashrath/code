#### Mirror Reflection

```java
class Solution {
    double EPS = 1e-6;

    public int mirrorReflection(int p, int q) {
        double x = 0, y = 0;
        double rx = p, ry = q;    

        // While it hasn't reached a receptor,...
        while (!( close(x, p) && (close(y, 0) || close(y, p))
                  || close(x, 0) && close (y, p) )) {
            // Want smallest t so that some x + rx, y + ry is 0 or p
            // x + rxt = 0, then t = -x/rx etc.
            double t = 1e9;
            if ((-x / rx) > EPS) t = Math.min(t, -x / rx);
            if ((-y / ry) > EPS) t = Math.min(t, -y / ry);
            if (((p-x) / rx) > EPS) t = Math.min(t, (p-x) / rx);
            if (((p-y) / ry) > EPS) t = Math.min(t, (p-y) / ry);

            x += rx * t;
            y += ry * t;

            if (close(x, p) || close(x, 0)) rx *= -1;
            if (close(y, p) || close(y, 0)) ry *= -1;
        }

        if (close(x, p) && close(y, p)) return 1;
        return close(x, p) ? 0 : 2;
    }

    public boolean close(double x, double y) {
        return Math.abs(x - y) < EPS;
    }
}```


```python
class Solution(object):
    def mirrorReflection(self, p, q):
        from fractions import Fraction as F

        x = y = 0
        rx, ry = p, q
        targets = [(p, 0), (p, p), (0, p)]

        while (x, y) not in targets:
            #Want smallest t so that some x + rx, y + ry is 0 or p
            #x + rxt = 0, then t = -x/rx etc.
            t = float('inf')
            for v in [F(-x,rx), F(-y,ry), F(p-x,rx), F(p-y,ry)]:
                if v > 0: t = min(t, v)

            x += rx * t
            y += ry * t

            #update rx, ry
            if x == p or x == 0: # bounced from east/west wall, so reflect on y axis
                rx *= -1
            if y == p or y == 0:
                ry *= -1

        return 1 if x==y==p else 0 if x==p else 2```


```java
class Solution {

    public int mirrorReflection(int p, int q) {
        int g = gcd(p, q);
        p /= g; p %= 2;
        q /= g; q %= 2;

        if (p == 1 && q == 1) return 1;
        return p == 1 ? 0 : 2;
    }

    public int gcd(int a, int b) {
        if (a == 0) return b;
        return gcd(b % a, a);
    }
}```


```python
class Solution(object):
    def mirrorReflection(self, p, q):
        from fractions import gcd
        g = gcd(p, q)
        p = (p / g) % 2
        q = (q / g) % 2

        return 1 if p and q else 0 if p else 2```

