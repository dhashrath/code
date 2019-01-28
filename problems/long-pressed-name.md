#### Long Pressed Name

```java
class Solution {
    public boolean isLongPressedName(String name, String typed) {
        Group g1 = groupify(name);
        Group g2 = groupify(typed);
        if (!g1.key.equals(g2.key))
            return false;

        for (int i = 0; i < g1.count.size(); ++i)
            if (g1.count.get(i) > g2.count.get(i))
                return false;
        return true;
    }

    public Group groupify(String S) {
        StringBuilder key = new StringBuilder();
        List<Integer> count = new ArrayList();
        int anchor = 0;
        int N = S.length();
        for (int i = 0; i < N; ++i) {
            if (i == N-1 || S.charAt(i) != S.charAt(i+1)) { // end of group
                key.append(S.charAt(i));
                count.add(i - anchor + 1);
                anchor = i+1;
            }
        }

        return new Group(key.toString(), count);
    }
}

class Group {
    String key;
    List<Integer> count;
    Group(String k, List<Integer> c) {
        key = k;
        count = c;
    }
}```


```python
class Solution(object):
    def isLongPressedName(self, name, typed):
        g1 = [(k, len(list(grp))) for k, grp in itertools.groupby(name)]
        g2 = [(k, len(list(grp))) for k, grp in itertools.groupby(typed)]
        if len(g1) != len(g2):
            return False

        return all(k1 == k2 and v1 <= v2
                   for (k1,v1), (k2,v2) in zip(g1, g2))```


```java
class Solution {
    public boolean isLongPressedName(String name, String typed) {
        int j = 0;
        for (char c: name.toCharArray()) {
            if (j == typed.length())
                return false;

            // If mismatch...
            if (typed.charAt(j) != c) {
                // If it's the first char of the block, ans is false.
                if (j==0 || typed.charAt(j-1) != typed.charAt(j))
                    return false;

                // Discard all similar chars
                char cur = typed.charAt(j);
                while (j < typed.length() && typed.charAt(j) == cur)
                    j++;

                // If next isn't a match, ans is false.
                if (j == typed.length() || typed.charAt(j) != c)
                    return false;
            }

            j++;
        }

        return true;
    }
}```


```python
class Solution(object):
    def isLongPressedName(self, name, typed):
        j = 0
        for c in name:
            if j == len(typed):
                return False

            # If mismatch...
            if typed[j] != c:
                # If it's the first char of the block, ans is False.
                if (j == 0) or (typed[j-1] != typed[j]):
                    return False

                # Discard all similar chars.
                cur = typed[j]
                while j < len(typed) and typed[j] == cur:
                    j += 1

                # If next isn't a match, ans is False.
                if j == len(typed) or typed[j] != c:
                    return False

            j += 1

        return True```

