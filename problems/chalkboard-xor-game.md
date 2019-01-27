

---
#### Approach #1: Mathematical [Accepted]

**Intuition and Algorithm**

As in the problem statement, if the `XOR` of the entire array is `0`, then Alice wins.

If the `XOR` condition is never triggered, then clearly Alice wins if and only if there are an even number of elements, as every player always has a move.

Now for the big leap in intuition.  Actually, Alice always has a move when there are an even number of elements.  If $$ S = x_1 \oplus x_2 \oplus \cdots x_n \neq 0 $$, but there are no possible moves ($$ S \oplus x_i = 0 $$), then $$(S \oplus x_1) \oplus (S \oplus x_2) \oplus \cdots \oplus (S \oplus x_n) = (S \oplus \cdots \oplus S) \oplus (x_1 \oplus x_2 \oplus \cdots \oplus x_n) = 0 \oplus S \neq 0$$, a contradiction.

Similarly, if there are an odd number of elements, then Bob always faces an even number of elements, and has a move.  So the answer is just the parity of the number of elements in the array.

Those that are familiar with the Sprague-Grundy theorem may know that this game is a misère-form game, meaning the theorem does not apply, and giving a big hint that there may exist a simpler solution.



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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `nums`.

* Space Complexity: $$O(1)$$.

---

Analysis written by: [@awice](https://leetcode.com/awice).
