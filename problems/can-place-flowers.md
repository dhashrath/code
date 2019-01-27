

## Solution

---
#### Approach #1 Single Scan [Accepted]

The solution is very simple. We can find out the extra maximum number of flowers, $$count$$, that can be planted for the given $$flowerbed$$ arrangement. To do so, we can traverse over all the elements of the $$flowerbed$$ and find out those elements which are 0(implying an empty position). For every such element, we check if its both adjacent positions are also empty. If so, we can plant a flower at the current position without violating the no-adjacent-flowers-rule. For the first and last elements, we need not check the previous and the next adjacent positions respectively.

If the $$count$$ obtained is greater than or equal to $$n$$, the required number of flowers to be planted, we can plant $$n$$ flowers in the empty spaces, otherwise not.


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

* Time complexity : $$O(n)$$. A single scan of the $$flowerbed$$ array of size $$n$$ is done.

* Space complexity : $$O(1)$$. Constant extra space is used.

---
#### Approach #2 Optimized [Accepted]

**Algorithm**

Instead of finding the maximum value of $$count$$ that can be obtained, as done in the last approach, we can stop the process of checking the positions for planting the flowers as soon as $$count$$ becomes equal to $$n$$. Doing this leads to an optimization of the first approach. If $$count$$ never becomes equal to $$n$$, $$n$$ flowers can't be planted at the empty positions.


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

* Time complexity : $$O(n)$$. A single scan of the $$flowerbed$$ array of size $$n$$ is done.

* Space complexity : $$O(1)$$. Constant extra space is used.

---
Analysis written by: [@vinod23](https://leetcode.com/vinod23)
