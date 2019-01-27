

#### Approach 1: Categorize by Sorted String

**Intuition**

Two strings are anagrams if and only if their sorted strings are equal.

**Algorithm**

Maintain a map `ans : {String -> List}` where each key $$\text{K}$$ is a sorted string, and each value is the list of strings from the initial input that when sorted, are equal to $$\text{K}$$.

In Java, we will store the key as a string, eg. `code`.  In Python, we will store the key as a hashable tuple, eg. `('c', 'o', 'd', 'e')`.

![Anagrams](../Figures/49_groupanagrams1.png)


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

* Time Complexity:  $$O(NK \log K)$$, where $$N$$ is the length of `strs`, and $$K$$ is the maximum length of a string in `strs`.  The outer loop has complexity $$O(N)$$ as we iterate through each string.  Then, we sort each string in $$O(K \log K)$$ time.

* Space Complexity: $$O(NK)$$, the total information content stored in `ans`.
<br />
<br />
---
#### Approach 2: Categorize by Count

**Intuition**

Two strings are anagrams if and only if their character counts (respective number of occurrences of each character) are the same.

**Algorithm**

We can transform each string $$\text{s}$$ into a character count, $$\text{count}$$, consisting of 26 non-negative integers representing the number of $$\text{a}$$'s, $$\text{b}$$'s, $$\text{c}$$'s, etc.  We use these counts as the basis for our hash map.

In Java, the hashable representation of our count will be a string delimited with '**#**' characters.  For example, `abbccc` will be `#1#2#3#0#0#0...#0` where there are 26 entries total.  In python, the representation will be a tuple of the counts.  For example, `abbccc` will be `(1, 2, 3, 0, 0, ..., 0)`, where again there are 26 entries total.

![Anagrams](../Figures/49_groupanagrams2.png)


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

* Time Complexity:  $$O(NK)$$, where $$N$$ is the length of `strs`, and $$K$$ is the maximum length of a string in `strs`.  Counting each string is linear in the size of the string, and we count every string.

* Space Complexity: $$O(NK)$$, the total information content stored in `ans`.