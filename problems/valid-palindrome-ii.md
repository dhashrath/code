

#### Approach #1: Brute Force [Time Limit Exceeded]

**Intuition and Algorithm**

For each index `i` in the given string, let's remove that character, then check if the resulting string is a palindrome.  If it is, (or if the original string was a palindrome), then we'll return `true`


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

* Time Complexity: $$O(N^2)$$ where $$N$$ is the length of the string.  We do the following $$N$$ times: create a string of length $$N$$ and iterate over it.

* Space Complexity: $$O(N)$$, the space used by our candidate answer.

---
#### Approach #2: Greedy [Accepted]

**Intuition**

If the beginning and end characters of a string are the same (ie. `s[0] == s[s.length - 1]`), then whether the inner characters are a palindrome (`s[1], s[2], ..., s[s.length - 2]`) uniquely determines whether the entire string is a palindrome.

**Algorithm**

Suppose we want to know whether `s[i], s[i+1], ..., s[j]` form a palindrome.  If `i >= j` then we are done.  If `s[i] == s[j]` then we may take `i++; j--`.  Otherwise, the palindrome must be either `s[i+1], s[i+2],  ..., s[j]` or `s[i], s[i+1], ..., s[j-1]`, and we should check both cases.


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

* Time Complexity: $$O(N)$$ where $$N$$ is the length of the string.  Each of two checks of whether some substring is a palindrome is $$O(N)$$.

* Space Complexity: $$O(1)$$ additional complexity.  Only pointers were stored in memory.

---

Analysis written by: [@awice](https://leetcode.com/awice)
