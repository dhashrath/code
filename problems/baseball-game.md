#### Approach #1: Stack [Accepted]

**Intuition and Algorithm**

Let's maintain the value of each valid round on a stack as we process the data.  A stack is ideal since we only deal with operations involving the last or second-last valid round.


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

* Time Complexity: $$O(N)$$, where $$N$$ is the length of `ops`.  We parse through every element in the given array once, and do $$O(1)$$ work for each element.

* Space Complexity: $$O(N)$$, the space used to store our `stack`.

---

Analysis written by: [@awice](https://leetcode.com/awice)
