

## Solution

---
#### Approach #1 Simple Solution[Accepted]

The first method is really simple. We simply split up the given string based on whitespaces and put the individual words in an array of strings. Then, we reverse each individual string and concatenate the result. We return the result after removing the additional whitespaces at the end.



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

* Time complexity : $$O(n)$$. where $$n$$ is the length of the string.

* Space complexity : $$O(n)$$. $$res$$ of size $$n$$ is used.

---
#### Approach #2 Without using pre-defined split and reverse function [Accepted]

**Algorithm**

We can create our own split and reverse function. Split function splits the string based on the delimiter " "(space) and returns the array of words. Reverse function returns the string after reversing the characters.




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

* Time complexity : $$O(n)$$. where $$n$$ is the length of the string.
* Space complexity : $$O(n)$$. $$res$$ of size $$n$$ is used.

---
#### Approach #3 Using StringBuilder and reverse method [Accepted]

**Algorithm**

Instead of using split method, we can use temporary string $$word$$ to store the word. We simply append the characters to the $$word$$ until `' '` character is not found. On getting `' '` we append the reverse of the $$word$$ to the resultant string $$result$$. Also after completion of loop , we still have to append the $$reverse$$ of the $$word$$(last word) to the $$result$$ string. 

Below code is inspired by [@ApolloX](http://leetcode.com/apolloX).



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

* Time complexity : $$O(n)$$. Single loop upto $$n$$ is there, where $$n$$ is the length of the string.
* Space complexity : $$O(n)$$. $$result$$ and $$word$$ size will grow upto $$n$$.

---
Analysis written by: [@vinod23](https://leetcode.com/vinod23)
