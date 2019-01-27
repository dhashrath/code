

## Solution

---
#### Approach #1 Brute Force [Time Limit Exceeded]

The simplest solution would be to consider every possible combination of integers $$a$$ and $$b$$ and check if the sum of their squares equals $$c$$. Now, both $$a$$ and $$b$$ can lie within the range $$(0,\sqrt{c})$$. Thus, we need to check for the values of $$a$$ and $$b$$ in this range only.


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

* Time complexity : $$O(c)$$. Two loops upto $$\sqrt{c}$$. Here, $$c$$ refers to the given integer(sum of squares).

* Space complexity : $$O(1)$$. Constant extra space is used.

---
#### Approach #2 Better Brute Force [Time Limit Exceeded]

We can improve the last solution, if we make the following observation. For any particular $$a$$ chosen, the value of $$b$$ required to satisfy the equation $$a^2 + b^2 = c$$ will be such that $$b^2 = c - a^2$$. Thus, we need to traverse over the range $$(0, \sqrt{c})$$ only for considering the various values of $$a$$. For every current value of $$a$$ chosen, we can determine the corresponding $$b^2$$ value and check if it is a perfect square or not. If it happens to be a perfect square, $$c$$ is a sum of squares of two integers, otherwise not.

Now, to determine, if the number $$c - a^2$$ is a perfect square or not, we can make use of the following theorem: "The square of $$n^{th}$$ positive integer can be represented as a sum of first $$n$$ odd positive integers." Or in mathematical terms:

$$n^2 = 1 + 3 + 5 + ... + (2*n-1) = \sum_{1}^{n} (2*i - 1)$$.

To look at the proof of this statement, look at the L.H.S. of the above statement.

$$1 + 3 + 5 + ... + (2*n-1)=$$

$$(2*1-1) + (2*2-1) + (2*3-1) + ... + (2*n-1)=$$

$$2*(1+2+3+....+n) - (1+1+...n times)=$$

$$2*n*(n+1)/2 - n=$$

$$n*(n+1) - n=$$

$$n^2 + n - n = n^2$$

This completes the proof of the above statement.




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

* Time complexity : $$O(c)$$. The total number of times the $$sum$$ is updated is: $$1+2+3+...(\sqrt{c} times) = \sqrt{c}(\sqrt{c}+1)/2 = O(c)$$.

* Space complexity : $$O(1)$$. Constant extra space is used.

---
#### Approach #3 Using sqrt function [Accepted]

**Algorithm**

Instead of finding if $$c - a^2$$ is a perfect square using sum of odd numbers, as done in the last approach, we can make use of the inbuilt $$sqrt$$ function and check if $$\sqrt{c - a^2}$$ turns out to be an integer. If it happens for any value of $$a$$ in the range $$[0, \sqrt{c}]$$, we can return a True value immediately.


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

* Time complexity : $$O\big(\sqrt{c}log(c)\big)$$. We iterate over $$\sqrt{c}$$ values for choosing $$a$$. For every $$a$$ chosen, finding square root of $$c - a^2$$ takes $$O\big(log(c)\big)$$ time in the worst case.

* Space complexity : $$O(1)$$. Constant extra space is used.

---
#### Approach #4 Using Binary Search [Accepted]

**Algorithm**

Another method to check if $$c - a^2$$ is a perfect square, is by making use of Binary Search. The method remains same as that of a typical Binary Search to find a number.
The only difference lies in that we need to find an integer, $$mid$$ in the range $$[0, c - a^2]$$, such that this number is the square root of $$c - a^2$$.
Or in other words, we need to find an integer, $$mid$$, in the range $$[0, c - a^2]$$, such that $$mid$$x$$mid = c - a^2$$.

The following animation illustrates the search process for a particular value of $$c - a^2 = 36$$.

!?!../Documents/633_Sum_of_Squares.json:1000,563!?!



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

* Time complexity : $$O\big(\sqrt{c}log(c)\big)$$. Binary search taking $$O\big(log(c)\big)$$ in the worst case is done for $$\sqrt{c}$$ values of $$a$$.

* Space complexity : $$O(log(c))$$. Binary Search will take $$O(log(c))$$ space.

---
#### Approach #5 Fermat Theorem [Accepted]:

**Algorithm**

This approach is based on the following statement, which is based on Fermat's Theorem:

"Any positive number $$n$$ is expressible as a sum of two squares if and only if the prime factorization of $$n$$, every prime of the form $$(4k+3)$$ occurs an even number of times."

By making use of the above theorem, we can directly find out if the given number $$c$$ can be expressed as a sum of two squares.

To do so we simply find all the prime factors of the given number $$c$$, which could range from $$[2,\sqrt{c}]$$ along with the count of those factors, by repeated division. 
If at any step, we find out that the number of occurences of any prime factor of the form $$(4k+3)$$ occurs an odd number of times, we can return a False value.

In case, $$c$$ itself is a prime number, it won't be divisible by any of the primes in the $$[2,\sqrt{c}]$$. Thus, we need to check if $$c$$ can be expressed in the form of
$$4k+3$$. If so, we need to return a False value, indicating that this prime occurs an odd number(1) of times. 

Otherwise, we can return a True value.

The proof of this theorem includes the knowledge of advanced mathematics and is beyond the scope of this article. However, interested reader can refer to [this](http://wstein.org/edu/124/lectures/lecture21/lecture21/node2.html) documentation.


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

* Time complexity : $$O\big(\sqrt{c}log(c)\big)$$. We find the factors of $$c$$ and their count using repeated division. We check for the factors in the range $$[0, \sqrt{c}]$$.
The maximum number of times a factor can occur(repeated division can be done) is $$log(n)$$(considering 2 as the only factor, $$c=2^x$$. Thus, $$x=log(c)$$).

* Space complexity : $$O(1)$$. Constant space is used.

---

Analysis written by: [@vinod23](https://leetcode.com/vinod23)