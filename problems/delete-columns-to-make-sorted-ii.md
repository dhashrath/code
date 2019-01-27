

## Solution
---
#### Approach 1: Greedy

**Intuition**

Instead of thinking about column deletions, let's think about which columns we will keep in the final answer.

If the first column isn't lexicographically sorted, we have to delete it.

Otherwise, we will argue that we can keep this first column without consequence.  There are two cases:

* If we don't keep the first column, then the final rows of the answer all have to be sorted.

* If we do keep the first column, then the final rows of the answer (minus the first column) only have to be sorted if they share the same first letter (coming from the first column).

  The above statement is hard to digest, so let's use an example:

  Say we have `A = ["axx","ayy","baa","bbb","bcc"]`.  When we keep the first column, the final rows are `R = ["xx","yy","aa","bb","cc"]`, and instead of the requirement that these all have to be sorted (ie. `R[0] <= R[1] <= R[2] <= R[3] <= R[4]`), we have a weaker requirement that they only have to be sorted if they share the same first letter of the first column, (ie. `R[0] <= R[1] and R[2] <= R[3] <= R[4]`).

Now, we applied this argument only for the first column, but it actually works for every column we could consider taking.  If we can't take a column, we have to delete it.  Otherwise, we take it because it can only make adding subsequent columns easier.

**Algorithm**

All our effort has led us to a simple algorithmic idea.

Start with no columns kept.  For each column, if we could keep it and have a valid answer, keep it - otherwise delete it.



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

* Time Complexity:  $$O(NW^2)$$, where $$N$$ is the length of `A`, and $$W$$ is the length of `A[i]`.

* Space Complexity:  $$O(NW)$$.
<br />
<br />


---
#### Approach 2: Greedy with Optimizations

**Explanation**

It is also possible to implement the solution in *Approach 1* without using as much time and space.

The key idea is that we will record the "cuts" that each column makes.  In our first example from *Approach 1* with `A = ["axx","ayy","baa","bbb","bcc"]` (and `R` defined as in Approach 1), the first column cuts our condition from `R[0] <= R[1] <= R[2] <= R[3] <= R[4]` to `R[0] <= R[1]` and `R[2] <= R[3] <= R[4]`.  That is, the boundary `"a" == column[1] != column[2] == "b"` has 'cut' one of the conditions for `R` out.

At a high level, our algorithm depends on evaluating whether adding a new column will keep all the rows sorted.  By maintaining information about these cuts, we only need to compare characters in the newest column.



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

* Time Complexity:  $$O(NW)$$, where $$N$$ is the length of `A`, and $$W$$ is the length of `A[i]`.

* Space Complexity:  $$O(N)$$ in additional space complexity.  (In Python, `zip(*A)` uses $$O(NW)$$ space.)
<br />
<br />


---
Analysis written by: [@awice](https://leetcode.com/awice).
