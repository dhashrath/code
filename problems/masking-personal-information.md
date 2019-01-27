

---
#### Approach #1: Direct [Accepted]

**Intuition and Algorithm**

We perform the algorithm as described.

First, to check if information is an email, we check whether it contains a `'@'`.  (There are many different tests: we could check for letters versus digits, for example.)

If we have an email, we should replace the first name with the first letter of that name, followed by 5 asterisks, followed by the last letter of that name.

If we have a phone number, we should collect all the digits and then format it according to the description.


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

* Time Complexity:  $$O(1)$$, if we consider the length of `S` as bounded by a constant.

* Space Complexity: $$O(1)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice).
