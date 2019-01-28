#### Chalkboard XOR Game

```java
class Solution {
    public boolean xorGame(int[] nums) {
      int x = 0;
      for (int v : nums) x ^= v;
      return x == 0 || nums.length % 2 == 0;
    }
}```


```python
class Solution(object):
    def xorGame(self, nums):
        return reduce(operator.xor, nums) == 0 or len(nums) % 2 == 0```

