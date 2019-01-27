

---
#### Approach #1: Adjacent Symbols [Accepted]

**Intuition**

Between every group of vertical dominoes (`'.'`), we have up to two non-vertical dominoes bordering this group.  Since additional dominoes outside this group do not affect the outcome, we can analyze these situations individually: there are 9 of them (as the border could be empty). Actually, if we border the dominoes by `'L'` and `'R'`, there are only 4 cases.  We'll write new letters between these symbols depending on each case.

**Algorithm**

Continuing our explanation, we analyze cases:

* If we have say `"A....B"`, where A = B, then we should write `"AAAAAA"`.

* If we have `"R....L"`, then we will write `"RRRLLL"`, or `"RRR.LLL"` if we have an odd number of dots.  If the initial symbols are at positions `i` and `j`, we can check our distance `k-i` and `j-k` to decide at position `k` whether to write `'L'`, `'R'`, or `'.'`.

* (If we have `"L....R"` we don't do anything.  We can skip this case.)


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

* Time and Space Complexity:  $$O(N)$$, where $$N$$ is the length of `dominoes`.

---
#### Approach #2: Calculate Force [Accepted]

**Intuition**

We can calculate the net force applied on every domino.  The forces we care about are how close a domino is to a leftward `'R'`, and to a rightward `'L'`: the closer we are, the stronger the force.

**Algorithm**

Scanning from left to right, our force decays by 1 every iteration, and resets to `N` if we meet an `'R'`, so that `force[i]` is higher (than `force[j]`) if and only if `dominoes[i]` is closer (looking leftward) to `'R'` (than `dominoes[j]`).

Similarly, scanning from right to left, we can find the force going rightward (closeness to `'L'`).

For some domino `answer[i]`, if the forces are equal, then the answer is `'.'`.  Otherwise, the answer is implied by whichever force is stronger.

**Example**

Here is a worked example on the string `S = 'R.R...L'`:  We find the force going from left to right is `[7, 6, 7, 6, 5, 4, 0]`.  The force going from right to left is `[0, 0, 0, -4, -5, -6, -7]`.  Combining them (taking their vector addition), the combined force is `[7, 6, 7, 2, 0, -2, -7]`, for a final answer of `RRRR.LL`.


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

* Time and Space Complexity:  $$O(N)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice).
