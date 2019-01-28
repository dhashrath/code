#### Smallest Range I

```java
class Solution {
    public int smallestRangeI(int[] A, int K) {
        int min = A[0], max = A[0];
        for (int x: A) {
            min = Math.min(min, x);
            max = Math.max(max, x);
        }
        return Math.max(0, max - min - 2*K);
    }
}```


```python
class Solution(object):
    def smallestRangeI(self, A, K):
        return max(0, max(A) - min(A) - 2*K)```

