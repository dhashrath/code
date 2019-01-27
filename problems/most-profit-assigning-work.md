

---
#### Approach #1: Sorting Events [Accepted]

**Intuition**

We can consider the workers in any order, so let's process them in order of skill.

If we processed all jobs with lower skill first, then the profit is just the most profitable job we have seen so far.

**Algorithm**

We can use a "two pointer" approach to process jobs in order.  We will keep track of `best`, the maximum profit seen.

For each worker with a certain `skill`, after processing all jobs with lower or equal difficulty, we add `best` to our answer.


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

* Time Complexity:  $$O(N \log N + Q \log Q)$$, where $$N$$ is the number of jobs, and $$Q$$ is the number of people.

* Space Complexity: $$O(N)$$, the additional space used by `jobs`.

---

Analysis written by: [@awice](https://leetcode.com/awice).
