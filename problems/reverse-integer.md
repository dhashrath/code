

## Solution
---

#### Approach 1: Pop and Push Digits & Check before Overflow

**Intuition**

We can build up the reverse integer one digit at a time.
While doing so, we can check beforehand whether or not appending another digit would cause overflow.

**Algorithm**

Reversing an integer can be done similarly to reversing a string.

We want to repeatedly "pop" the last digit off of $$x$$ and "push" it to the back of the $$\text{rev}$$. In the end, $$\text{rev}$$ will be the reverse of the $$x$$.

To "pop" and "push" digits without the help of some auxiliary stack/array, we can use math.

```cpp
//pop operation:
pop = x % 10;
x /= 10;

//push operation:
temp = rev * 10 + pop;
rev = temp;
```

However, this approach is dangerous, because the statement $$\text{temp} = \text{rev} \cdot 10 + \text{pop}$$ can cause overflow.

Luckily, it is easy to check beforehand whether or this statement would cause an overflow.

To explain, lets assume that $$\text{rev}$$ is positive.

1. If $$temp = \text{rev} \cdot 10 + \text{pop}$$ causes overflow, then it must be that $$\text{rev} \geq \frac{INTMAX}{10}$$
2. If $$\text{rev} > \frac{INTMAX}{10}$$, then $$temp = \text{rev} \cdot 10 + \text{pop}$$ is guaranteed to overflow.
3. If $$\text{rev} == \frac{INTMAX}{10}$$, then $$temp = \text{rev} \cdot 10 + \text{pop}$$ will overflow if and only if $$\text{pop} > 7$$

Similar logic can be applied when $$\text{rev}$$ is negative.


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

* Time Complexity: $$O(\log(x))$$. There are roughly $$\log_{10}(x)$$ digits in $$x$$.
* Space Complexity: $$O(1)$$.
