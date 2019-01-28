#### Monotone Increasing Digits

```java
class Solution {
    public int monotoneIncreasingDigits(int N) {
        String S = String.valueOf(N);
        String ans = "";
        search: for (int i = 0; i < S.length(); ++i) {
            for (char d = '1'; d <= '9'; ++d) {
                if (S.compareTo(ans + repeat(d, S.length() - i)) < 0) {
                    ans += (char) (d - 1);
                    continue search;
                }
            }
            ans += '9';
        }
        return Integer.parseInt(ans);
    }

    public String repeat(char c, int count) {
        StringBuilder sb = new StringBuilder(count);
        for (int i = 0; i < count; ++i) sb.append(c);
        return sb.toString();
    }
}```


```python
class Solution(object):
    def monotoneIncreasingDigits(self, N):
        digits = []
        A = map(int, str(N))
        for i in xrange(len(A)):
            for d in xrange(1, 10):
                if digits + [d] * (len(A)-i) > A:
                    digits.append(d-1)
                    break
            else:
                digits.append(9)

        return int("".join(map(str, digits)))```


```java
class Solution {
    public int monotoneIncreasingDigits(int N) {
        char[] S = String.valueOf(N).toCharArray();
        int i = 1;
        while (i < S.length && S[i-1] <= S[i]) i++;
        while (0 < i && i < S.length && S[i-1] > S[i]) S[--i]--;
        for (int j = i+1; j < S.length; ++j) S[j] = '9';

        return Integer.parseInt(String.valueOf(S));
    }
}```


```python
class Solution(object):
    def monotoneIncreasingDigits(self, N):
        S = list(str(N))
        i = 1
        while i < len(S) and S[i-1] <= S[i]:
            i += 1
        while 0 < i < len(S) and S[i-1] > S[i]:
            S[i-1] = str(int(S[i-1]) - 1)
            i -= 1
        S[i+1:] = '9' * (len(S) - i-1)
        return int("".join(S))```

