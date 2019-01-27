

## Solution

---
#### Approach #1: Brute Force [Accepted]

**Intuition**

The number only has at most 8 digits, so there are only $${}^{8}\text{C}_{2}$$ = 28 available swaps.  We can easily brute force them all.

**Algorithm**

We will store the candidates as lists of length $$\text{len(num)}$$.  For each candidate swap with positions $$\text{(i, j)}$$, we swap the number and record if the candidate is larger than the current answer, then swap back to restore the original number.

The only detail is possibly to check that we didn't introduce a leading zero.  We don't actually need to check it, because our original number doesn't have one.


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

* Time Complexity:  $$O(N^3)$$, where $$N$$ is the total number of digits in the input number.  For each pair of digits, we spend up to $$O(N)$$ time to compare the final sequences.

* Space Complexity: $$O(N)$$, the information stored in $$\text{A}$$.

---

#### Approach #2: Greedy [Accepted]

**Intuition**

At each digit of the input number in order, if there is a larger digit that occurs later, we know that the best swap must occur with the digit we are currently considering.

**Algorithm**

We will compute $$\text{last[d] = i}$$, the index $$\text{i}$$ of the last occurrence of digit $$\text{d}$$ (if it exists).

Afterwards, when scanning the number from left to right, if there is a larger digit in the future, we will swap it with the largest such digit; if there are multiple such digits, we will swap it with the one that occurs the latest.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the total number of digits in the input number.  Every digit is considered at most once.

* Space Complexity: $$O(1)$$.  The additional space used by $$\text{last}$$ only has up to 10 values.

---
Analysis written by: [@awice](https://leetcode.com/awice)
