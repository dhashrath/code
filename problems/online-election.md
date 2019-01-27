

## Solution
---
#### Approach 1: List of Lists + Binary Search

**Intuition and Algorithm**

We can store the votes in a list `A` of lists of votes.  Each vote has a person and a timestamp, and `A[count]` is a list of the `count`-th votes received for that person.

Then, `A[i][0]` and `A[i]` are monotone increasing, so we can binary search on them to find the most recent vote by time.


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

* Time Complexity:  $$O(N + Q \log^2 N)$$, where $$N$$ is the number of votes, and $$Q$$ is the number of queries.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---
#### Approach 2: Precomputed Answer + Binary Search

**Intuition and Algorithm**

As the votes come in, we can remember every event `(winner, time)` when the winner changes.  After, we have a sorted list of these events that we can binary search for the answer.


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

* Time Complexity:  $$O(N + Q \log N)$$, where $$N$$ is the number of votes, and $$Q$$ is the number of queries.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
