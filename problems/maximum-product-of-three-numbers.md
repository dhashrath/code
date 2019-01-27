

## Solution

---
#### Approach #1 Brute Force [Time Limit Exceeded]

The simplest solution is to consider every triplet out of the given $$nums$$ array and check their product and find out the maximum product out of them.

**Complexity Analysis**

* Time complexity : $$O(n^3)$$. We need to consider every triplet from $$nums$$ array of length $$n$$.

* Space complexity : $$O(1)$$. Constant extra space is used.

---
#### Approach #2 Using Sorting [Accepted]

**Algorithm**

Another solution could be to sort the given $$nums$$ array(in ascending order) and find out the product of the last three numbers. 

But, we can note that this product will be maximum only if all the numbers in $$nums$$ array are positive. But, in the given problem statement, negative elements could exist as well. 

Thus, it could also be possible that two negative numbers lying at the left extreme end could also contribute to lead to a larger product if the third number in the triplet being considered is the largest positive number in the $$nums$$ array. 

Thus, either the product $$nums[0]$$x$$nums[1]$$x$$nums[n-1]$$ or $$nums[n-3]$$x$$nums[n-2]$$x$$nums[n-1]$$ will give the required result. Thus, we need to find the larger one from out of these values.


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

* Time complexity : $$O\big(nlog(n)\big)$$. Sorting the $$nums$$ array takes $$nlog(n)$$ time.

* Space complexity : $$O(log(n)))$$. Sorting takes O(logn) space.

---
#### Approach #3 Single Scan [Accepted]

**Algorithm**

We need not necessarily sort the given $$nums$$ array to find the maximum product. Instead, we can only find the required 2 smallest values($$min1$$ and $$min2$$) and the three largest values($$max1, max2, max3$$) in the $$nums$$ array, by iterating over the $$nums$$ array only once. 

At the end, again we can find out the larger value out of $$min1$$x$$min2$$x$$max1$$ and $$max1$$x$$max2$$x$$max3$$ to find the required maximum product.


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

* Time complexity : $$O(n)$$. Only one iteration over the $$nums$$ array of length $$n$$ is required.

* Space complexity : $$O(1)$$. Constant extra space is used.

---
Analysis written by: [@vinod23](https://leetcode.com/vinod23)
