#### Max Chunks To Make Sorted II

```java
class Solution {
    public int maxChunksToSorted(int[] arr) {
        Map<Integer, Integer> count = new HashMap();
        int ans = 0, nonzero = 0;

        int[] expect = arr.clone();
        Arrays.sort(expect);

        for (int i = 0; i < arr.length; ++i) {
            int x = arr[i], y = expect[i];

            count.put(x, count.getOrDefault(x, 0) + 1);
            if (count.get(x) == 0) nonzero--;
            if (count.get(x) == 1) nonzero++;

            count.put(y, count.getOrDefault(y, 0) - 1);
            if (count.get(y) == -1) nonzero++;
            if (count.get(y) == 0) nonzero--;

            if (nonzero == 0) ans++;
        }

        return ans;
    }
}```


```python
class Solution(object):
    def maxChunksToSorted(self, arr):
        count = collections.defaultdict(int)
        ans = nonzero = 0

        for x, y in zip(arr, sorted(arr))
            count[x] += 1
            if count[x] == 0: nonzero -= 1
            if count[x] == 1: nonzero += 1

            count[y] -= 1
            if count[y] == -1: nonzero += 1
            if count[y] == 0: nonzero -= 1

            if nonzero == 0: ans += 1

        return ans```


```java
class Solution {
    public int maxChunksToSorted(int[] arr) {
        Map<Integer, Integer> count = new HashMap();
        List<Pair> counted = new ArrayList();
        for (int x: arr) {
            count.put(x, count.getOrDefault(x, 0) + 1);
            counted.add(new Pair(x, count.get(x)));
        }

        List<Pair> expect = new ArrayList(counted);
        Collections.sort(expect, (a, b) -> a.compare(b));

        Pair cur = counted.get(0);
        int ans = 0;
        for (int i = 0; i < arr.length; ++i) {
            Pair X = counted.get(i), Y = expect.get(i);
            if (X.compare(cur) > 0) cur = X;
            if (cur.compare(Y) == 0) ans++;
        }

        return ans;
    }
}

class Pair {
    int val, count;
    Pair(int v, int c) {
        val = v; count = c;
    }
    int compare(Pair that) {
        return this.val != that.val ? this.val - that.val : this.count - that.count;
    }
}```


```python
class Solution(object):
    def maxChunksToSorted(self, arr):
        count = collections.Counter()
        counted = []
        for x in arr:
            count[x] += 1
            counted.append((x, count[x]))

        ans, cur = 0, None
        for X, Y in zip(counted, sorted(counted)):
            cur = max(cur, X)
            if cur == Y:
                ans += 1
        return ans```

