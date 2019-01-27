

---
#### Approach #1: Cartesian Product [Accepted]

**Intuition and Algorithm**

For each place to put the comma, we separate the string into two fragments.  For example, with a string like `"1234"`, we could separate it into fragments `"1" and "234"`, `"12" and "34"`, or `"123"` and `"4"`.

Then, for each fragment, we have a choice of where to put the period, to create a list `make(...)` of choices.  For example, `"123"` could be made into `"1.23"`, `"12.3"`, or `"123"`.

Because of extranneous zeroes, we should ignore possibilities where the part of the fragment to the `left` of the decimal starts with `"0"` (unless it is exactly `"0"`), and ignore possibilities where the part of the fragment to the `right` of the decimal ends with `"0"`, as these are not allowed.

Note that this process could result in an empty answer, such as for the case `S = "(000)"`.


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

* Time Complexity:  $$O(N^3)$$, where $$N$$ is the length `S`.  We evaluate the sum $$O(\sum_k k(N-k))$$.

* Space Complexity: $$O(N^3)$$, to store the answer.

---

Analysis written by: [@awice](https://leetcode.com/awice).
