

## Solution
---
#### Approach 1: Group into Blocks

**Intuition and Algorithm**

For a string like `S = 'aabbbbccc'`, we can group it into blocks `groupify(S) = [('a', 2), ('b', 4), ('c', 3)]`, that consist of a *key* `'abc'` and a *count* `[2, 4, 3]`.

Then, the necessary and sufficient condition for `typed` to be a long-pressed version of `name` is that the keys are the same, and each entry of the count of `typed` is at least the entry for the count of `name`.

For example, `'aaleex'` is a long-pressed version of `'alex'`: because when considering the groups `[('a', 2), ('l', 1), ('e', 2), ('x', 1)]` and `[('a', 1), ('l', 1), ('e', 1), ('x', 1)]`, they both have the key `'alex'`, and the count `[2,1,2,1]` is at least `[1,1,1,1]` when making an element-by-element comparison `(2 >= 1, 1 >= 1, 2 >= 1, 1 >= 1)`.


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

* Time Complexity:  $$O(N+T)$$, where $$N, T$$ are the lengths of `name` and `typed`.

* Space Complexity:  $$O(N+T)$$.
<br />
<br />


---
#### Approach 2: Two Pointer

**Intuition**

As in *Approach 1*, we want to check the key and the count.  We can do this on the fly.

Suppose we read through the characters `name`, and eventually it doesn't match `typed`.

There are some cases for when we are allowed to skip characters of `typed`. Let's use a tuple to denote the case (`name`, `typed`):

* In a case like `('aab', 'aaaaab')`, we can skip the 3rd, 4th, and 5th `'a'` in `typed` because we have already processed an `'a'` in this block.

* In a case like `('a', 'b')`, we can't skip the 1st `'b'` in `typed` because we haven't processed anything in the current block yet.

**Algorithm**

This leads to the following algorithm:

* For each character in `name`, if there's a mismatch with the next character in `typed`:
    * If it's the first character of the block in `typed`, the answer is `False`.
    * Else, discard all similar characers of `typed` coming up.  The next (different) character coming must match.

Also, we'll keep track on the side of whether we are at the first character of the block.


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

* Time Complexity:  $$O(N+T)$$, where $$N, T$$ are the lengths of `name` and `typed`.

* Space Complexity:  $$O(1)$$ in additional space complexity.  (In Java, `.toCharArray` makes this $$O(N)$$, but this can be easily remedied.)
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
