

#### Approach #1: Convert to String [Accepted]

**Intuition and Algorithm**

Let's convert the given number into a string of binary digits.  Then, we should simply check that no two adjacent digits are the same.


```java
public class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length < 2)
            return nums.length;
        int[] up = new int[nums.length];
        int[] down = new int[nums.length];
        for (int i = 1; i < nums.length; i++) {
            for(int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    up[i] = Math.max(up[i],down[j] + 1);
                } else if (nums[i] < nums[j]) {
                    down[i] = Math.max(down[i],up[j] + 1);
                }
            }
        }
        return 1 + Math.max(down[nums.length - 1], up[nums.length - 1]);
    }
}```


**Complexity Analysis**

* Time Complexity: $$O(1)$$.  For arbitrary inputs, we do $$O(w)$$ work, where $$w$$ is the number of bits in `n`.  However, $$w \leq 32$$.

* Space complexity: $$O(1)$$, or alternatively $$O(w)$$.

---

#### Approach #2: Divide By Two [Accepted]

**Intuition and Algorithm**

We can get the last bit and the rest of the bits via `n % 2` and `n // 2` operations.  Let's remember `cur`, the last bit of `n`.  If the last bit ever equals the last bit of the remaining, then two adjacent bits have the same value, and the answer is `False`.  Otherwise, the answer is `True`.

Also note that instead of `n % 2` and `n // 2`, we could have used operators `n & 1` and `n >>= 1` instead.


```java
public class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length < 2)
            return nums.length;
        int[] up = new int[nums.length];
        int[] down = new int[nums.length];
        for (int i = 1; i < nums.length; i++) {
            for(int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    up[i] = Math.max(up[i],down[j] + 1);
                } else if (nums[i] < nums[j]) {
                    down[i] = Math.max(down[i],up[j] + 1);
                }
            }
        }
        return 1 + Math.max(down[nums.length - 1], up[nums.length - 1]);
    }
}```


**Complexity Analysis**

* Time Complexity: $$O(1)$$.  For arbitrary inputs, we do $$O(w)$$ work, where $$w$$ is the number of bits in `n`.  However, $$w \leq 32$$.

* Space complexity: $$O(1)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice)
