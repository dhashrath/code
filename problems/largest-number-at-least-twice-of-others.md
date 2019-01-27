



#### Approach #1: Linear Scan [Accepted]



**Intuition and Algorithm**



Scan through the array to find the unique largest element `m`, keeping track of it's index `maxIndex`.



Scan through the array again.  If we find some `x != m` with `m < 2*x`, we should return `-1`.



Otherwise, we should return `maxIndex`.




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



* Time Complexity: $$O(N)$$ where $$N$$ is the length of `nums`.



* Space Complexity: $$O(1)$$, the space used by our `int` variables.



---



Analysis written by: [@awice](https://leetcode.com/awice).
