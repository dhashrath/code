

#### Approach #1: Record Letters Seen [Accepted]

**Intuition and Algorithm**

Let's scan through `letters` and record if we see a letter or not.  We could do this with an array of size 26, or with a Set structure.

Then, for every next letter (starting with the letter that is one greater than the target), let's check if we've seen it.  If we have, it must be the answer.


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

* Time Complexity: $$O(N)$$, where $$N$$ is the length of `letters`.  We scan every element of the array.

* Space Complexity: $$O(1)$$, the maximum size of `seen`.

---
#### Approach #2: Linear Scan [Accepted]

**Intuition and Algorithm**

Since `letters` are sorted, if we see something larger when scanning form left to right, it must be the answer.  Otherwise, (since `letters` is non-empty), the answer is `letters[0]`.


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

* Time Complexity: $$O(N)$$, where $$N$$ is the length of `letters`.  We scan every element of the array.

* Space Complexity: $$O(1)$$, as we maintain only pointers.

---
#### Approach #3: Binary Search [Accepted]

**Intuition and Algorithm**

Like in *Approach #2*, we want to find something larger than target in a sorted array.  This is ideal for a *binary search*: Let's find the rightmost position to insert `target` into `letters` so that it remains sorted.

Our binary search (a typical one) proceeds in a number of rounds.  At each round, let's maintain the *loop invariant* that the answer must be in the interval `[lo, hi]`.  Let `mi = (lo + hi) / 2`.  If `letters[mi] <= target`, then we must insert it in the interval `[mi + 1, hi]`.  Otherwise, we must insert it in the interval `[lo, mi]`.

At the end, if our insertion position says to insert `target` into the last position `letters.length`, we return `letters[0]` instead.  This is what the modulo operation does.


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

* Time Complexity: $$O(\log N)$$, where $$N$$ is the length of `letters`.  We peek only at $$\log N$$ elements in the array.

* Space Complexity: $$O(1)$$, as we maintain only pointers.

---

Analysis written by: [@awice](https://leetcode.com/awice).
