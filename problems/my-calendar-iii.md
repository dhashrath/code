

#### Approach #1: Boundary Count [Accepted]

**Intuition and Algorithm**

When booking a new event `[start, end)`, count `delta[start]++` and `delta[end]--`.  When processing the values of `delta` in sorted order of their keys, the largest such value is the answer.

In Python, we sort the set each time instead, as there is no analog to *TreeMap* available.


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

* Time Complexity: $$O(N^2)$$, where $$N$$ is the number of events booked.  For each new event, we traverse `delta` in $$O(N)$$ time.  In Python, this is $$O(N^2 \log N)$$ owing to the extra sort step.

* Space Complexity: $$O(N)$$, the size of `delta`.

---

Analysis written by: [@awice](https://leetcode.com/awice).  Solution in Approach #2 inspired by [@cchao](https://discuss.leetcode.com/topic/111276/simplified-winner-s-solution).
