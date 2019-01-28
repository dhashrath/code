#### Remove 9

```java
class Solution {
    public int newInteger(int n) {
        return Integer.parseInt(Integer.toString(n, 9));
    }
}```


```python
class Solution(object):
    def newInteger(self, n):
        ans = ''
        while n:
            ans = str(n%9) + ans
            n /= 9
        return int(ans)```

