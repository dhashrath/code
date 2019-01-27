

---
#### Approach #1: Polynomial Class [Accepted]

**Intuition**

Keep a class `Poly` that knows a map from a list of free variables to a coefficient, and support operations on this class.

**Algorithm**

Each function is straightforward individually.  Let's list the functions we use:

* `Poly:add(this, that)` returns the result of `this + that`.
* `Poly:sub(this, that)` returns the result of `this - that`.
* `Poly:mul(this, that)` returns the result of `this * that`.
* `Poly:evaluate(this, evalmap)` returns the polynomial after replacing all free variables with constants as specified by `evalmap`.
* `Poly:toList(this)` returns the polynomial in the correct output format.

* `Solution::combine(left, right, symbol)` returns the result of applying the binary operator represented by `symbol` to `left` and `right`.
* `Solution::make(expr)` makes a new `Poly` represented by either the constant or free variable specified by `expr`.
* `Solution::parse(expr)` parses an expression into a new `Poly`.


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

* Time Complexity:  Let $$N$$ is the length of `expression` and $$M$$ is the length of `evalvars` and `evalints`.  With an expression like `(a + b) * (c + d) * (e + f) * ...` the complexity is $$O(2^N + M)$$.

* Space Complexity: $$O(N + M)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice).
