#### Bulb Switcher II

```java
class Solution {
    public int flipLights(int n, int m) {
        Set<Integer> seen = new HashSet();
        n = Math.min(n, 6);
        int shift = Math.max(0, 6-n);
        for (int cand = 0; cand < 16; ++cand) {
            int bcount = Integer.bitCount(cand);
            if (bcount % 2 == m % 2 && bcount <= m) {
                int lights = 0;
                if (((cand >> 0) & 1) > 0) lights ^= 0b111111 >> shift;
                if (((cand >> 1) & 1) > 0) lights ^= 0b010101 >> shift;
                if (((cand >> 2) & 1) > 0) lights ^= 0b101010 >> shift;
                if (((cand >> 3) & 1) > 0) lights ^= 0b100100 >> shift;
                seen.add(lights);
            }
        }
        return seen.size();
    }
}```


```python
def flipLights(self, n, m):
    seen = set()
    for cand in itertools.product((0, 1), repeat = 4):
        if sum(cand) % 2 == m % 2 and sum(cand) <= m:
            A = []
            for i in xrange(min(n, 3)):
                light = 1
                light ^= cand[0]
                light ^= cand[1] and i % 2
                light ^= cand[2] and i % 2 == 0
                light ^= cand[3] and i % 3 == 0
                A.append(light)
            seen.add(tuple(A))

    return len(seen)```


```java
class Solution {
    public int flipLights(int n, int m) {
        n = Math.min(n, 3);
        if (m == 0) return 1;
        if (m == 1) return n == 1 ? 2 : n == 2 ? 3 : 4;
        if (m == 2) return n == 1 ? 2 : n == 2 ? 4 : 7;
        return n == 1 ? 2 : n == 2 ? 4 : 8;
    }
}```


```python
class Solution(object):
    def flipLights(self, n, m):
        n = min(n, 3)
        if m == 0: return 1
        if m == 1: return [2, 3, 4][n-1]
        if m == 2: return [2, 4, 7][n-1]
        return [2, 4, 8][n-1]```

