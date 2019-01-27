

## Solution
---
#### Approach 1: HashMap

**Intuition and Algorithm**

We analyze the 3 cases that the algorithm needs to consider: when the query is an exact match, when the query is a match up to capitalization, and when the query is a match up to vowel errors.

In all 3 cases, we can use a hash table to query the answer.

* For the first case (exact match), we hold a set of words to efficiently test whether our query is in the set.
* For the second case (capitalization), we hold a hash table that converts the word from its lowercase version to the original word (with correct capitalization).
* For the third case (vowel replacement), we hold a hash table that converts the word from its lowercase version with the vowels masked out, to the original word.

The rest of the algorithm is careful planning and reading the problem carefully.


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

* Time Complexity:  $$O(\mathcal{C})$$, where $$\mathcal{C}$$ is the total *content* of `wordlist` and `queries`.

* Space Complexity:  $$O(\mathcal{C})$$.
<br />
<br />


---
Analysis written by: [@awice](https://leetcode.com/awice).
