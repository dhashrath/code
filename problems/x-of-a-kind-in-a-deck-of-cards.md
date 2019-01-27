

## Solution
---
#### Approach 1: Brute Force

**Intuition**

We can try every possible `X`.  

**Algorithm**

Since we divide the deck of `N` cards into say, `K` piles of `X` cards each, we must have `N % X == 0`.

Then, say the deck has `C_i` copies of cards with number `i`.  Each group with number `i` has `X` copies, so we must have `C_i % X == 0`.  These are necessary and sufficient conditions.


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

* Time Complexity:  $$O(N^2 \log \log N)$$, where $$N$$ is the number of cards.  It is outside the scope of this article to prove that the number of divisors of $$N$$ is bounded by $$O(N \log \log N)$$.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---
#### Approach 2: Greatest Common Divisor

**Intuition and Algorithm**

Again, say there are `C_i` cards of number `i`.  These must be broken down into piles of `X` cards each, ie. `C_i % X == 0` for all `i`.

Thus, `X` must divide the greatest common divisor of `C_i`.  If this greatest common divisor `g` is greater than `1`, then `X = g` will satisfy.  Otherwise, it won't.


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

* Time Complexity:  $$O(N \log^2 N)$$, where $$N$$ is the number of votes.  If there are $$C_i$$ cards with number $$i$$, then each `gcd` operation is naively $$O(\log^2 C_i)$$.  Better bounds exist, but are outside the scope of this article to develop.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
