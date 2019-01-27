

## Solution
---
#### Approach 1: Mathematical

**Intuition**

Say $$P = R^2$$ is a superpalindrome.

Because $$R$$ is a palindrome, the first half of the digits in $$R$$ determine $$R$$ up to two possibilities.  We can iterate through these digits: let $$k$$ be the first half of the digits in $$R$$.  For example, if $$k = 1234$$, then $$R = 1234321$$ or $$R = 12344321$$.  Each possibility has either an odd or an even number of digits in $$R$$.

Notice because $$P < 10^{18}$$, $$R < (10^{18})^{\frac{1}{2}} = 10^9$$, and $$R = k \| k'$$ (concatenation), where $$k'$$ is $$k$$ reversed (and also possibly truncated by one digit); so that $$k < 10^5 = \small\text{MAGIC}$$, our magic constant.

**Algorithm**

For each $$1 \leq k < \small\text{MAGIC}$$, let's create the associated palindrome $$R$$, and check whether $$R^2$$ is a palindrome.

We should handle the odd and even possibilities separately, as we would like to break early so as not to do extra work.

To check whether an integer is a palindrome, we could check whether it is equal to its reverse.  To create the reverse of an integer, we can do it digit by digit.


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

* Time Complexity:  $$O(W^{\frac{1}{4}} * \log W)$$, where $$W = 10^{18}$$ is our upper limit for $$R$$.  The $$\log W$$ term comes from checking whether each candidate is the root of a palindrome.

* Space Complexity:  $$O(\log W)$$, the space used to create the candidate palindrome.
<br />
<br />


---

Analysis written by: [@awice](https://leetcode.com/awice).
