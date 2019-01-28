#### Unique Letter String

```java
class Solution {
    Map<Character, List<Integer>> index;
    int[] peek;
    int N;

    public int uniqueLetterString(String S) {
        index = new HashMap();
        peek = new int[26];
        N = S.length();

        for (int i = 0; i < S.length(); ++i) {
            char c = S.charAt(i);
            index.computeIfAbsent(c, x-> new ArrayList<Integer>()).add(i);
        }

        long cur = 0, ans = 0;
        for (char c: index.keySet()) {
            index.get(c).add(N);
            index.get(c).add(N);
            cur += get(c);
        }

        for (char c: S.toCharArray()) {
            ans += cur;
            long oldv = get(c);
            peek[c - 'A']++;
            cur += get(c) - oldv;
        }
        return (int) ans % 1_000_000_007;
    }

    public long get(char c) {
        List<Integer> indexes = index.get(c);
        int i = peek[c - 'A'];
        return indexes.get(i+1) - indexes.get(i);
    }
}```


```python
class Solution(object):
    def uniqueLetterString(self, S):
        N = len(S)
        index = collections.defaultdict(list)
        peek = collections.defaultdict(int)
        for i, c in enumerate(S):
            index[c].append(i)
        for c in index:
            index[c].extend([N, N])

        def get(c):
            return index[c][peek[c] + 1] - index[c][peek[c]]

        ans = 0
        cur = sum(get(c) for c in index)
        for i, c in enumerate(S):
            ans += cur
            oldv = get(c)
            peek[c] += 1
            cur += get(c) - oldv
        return ans % (10**9 + 7)```


```java
class Solution {
    public int uniqueLetterString(String S) {
        Map<Character, List<Integer>> index = new HashMap();
        for (int i = 0; i < S.length(); ++i) {
            char c = S.charAt(i);
            index.computeIfAbsent(c, x-> new ArrayList<Integer>()).add(i);
        }

        long ans = 0;
        for (List<Integer> A: index.values()) {
            for (int i = 0; i < A.size(); ++i) {
                long prev = i > 0 ? A.get(i-1) : -1;
                long next = i < A.size() - 1 ? A.get(i+1) : S.length();
                ans += (A.get(i) - prev) * (next - A.get(i));
            }
        }

        return (int) ans % 1_000_000_007;
    }
}```


```python
class Solution(object):
    def uniqueLetterString(self, S):
        index = collections.defaultdict(list)
        for i, c in enumerate(S):
            index[c].append(i)

        ans = 0
        for A in index.values():
            A = [-1] + A + [len(S)]
            for i in xrange(1, len(A) - 1):
                ans += (A[i] - A[i-1]) * (A[i+1] - A[i])
        return ans % (10**9 + 7)```

