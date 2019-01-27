

---
#### Approach #1: Two Pointer [Accepted]

**Intuition**

Without loss of generality, a mountain can only start after the previous one ends.

This is because if it starts before the peak, it will be smaller than a mountain starting previous; and it is impossible to start after the peak.

**Algorithm**

For a starting index `base`, let's calculate the length of the longest mountain `A[base], A[base+1], ..., A[end]`.

If such a mountain existed, the next possible mountain will start at `base = end`; if it didn't, then either we reached the end, or we have `A[base] > A[base+1]` and we can start at `base + 1`.

**Example**

Here is a worked example on the array `A = [1, 2, 3, 2, 1, 0, 2, 3, 1]`:

<center>
    <img src="../Figures/845/diagram1.png" alt="Worked example of A = [1,2,3,2,1,0,2,3,1]" style="height: 150px"/>
</center>

<br>

`base` starts at `0`, and `end` travels using the first while loop to `end = 2` (`A[end] = 3`), the potential peak of this mountain.  After, it travels to `end = 5` (`A[end] = 0`) during the second while loop, and a candidate answer of 6 `(base = 0, end = 5)` is recorded.

Afterwards, base is set to `5` and the process starts over again, with `end = 7` the peak of the mountain, and `end = 8` the right boundary, and the candidate answer of 4 `(base = 5, end = 8)` being recorded.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `A`.

* Space Complexity:  $$O(1)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice).
