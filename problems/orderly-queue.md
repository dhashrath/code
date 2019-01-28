#### Orderly Queue

```java
class Solution {
    public String orderlyQueue(String S, int K) {
        if (K == 1) {
            String ans = S;
            for (int i = 0; i < S.length(); ++i) {
                String T = S.substring(i) + S.substring(0, i);
                if (T.compareTo(ans) < 0) ans = T;
            }
            return ans;
        } else {
            char[] ca = S.toCharArray();
            Arrays.sort(ca);
            return new String(ca);
        }
    }
}```


```python
class Solution(object):
    def orderlyQueue(self, S, K):
        if K == 1:
            return min(S[i:] + S[:i] for i in range(len(S)))
        return "".join(sorted(S))```

