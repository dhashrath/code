

#### Approach #1: Recursion [Accepted]

**Intuition and Algorithm**

Write a function `parse` that parses the formula from index `i`, returning a map `count` from names to multiplicities (the number of times that name is recorded).

We will put `i` in global state: our `parse` function increments `i` throughout any future calls to `parse`.

* When we see a `'('`, we will parse whatever is inside the brackets (up to the closing ending bracket) and add it to our count.

* Otherwise, we should see an uppercase character: we will parse the rest of the letters to get the name, and add that (plus the multiplicity if there is one.)

* At the end, if there is a final multiplicity (representing the multiplicity of a bracketed sequence), we'll multiply our answer by this.


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

* Time Complexity: $$O(N^2)$$, where $$N$$ is the length of the formula.  It is $$O(N)$$ to parse through the formula, but each of $$O(N)$$ multiplicities after a bracket may increment the count of each name in the formula (inside those brackets), leading to an $$O(N^2)$$ complexity.

* Space Complexity: $$O(N)$$.  We aren't recording more intermediate information than what is contained in the formula.

---
#### Approach #2: Stack [Accepted]

**Intuition and Algorithm**

Instead of recursion, we can simulate the call stack by using a stack of `count`s directly.


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

* Time Complexity $$O(N^2)$$, and Space Complexity $$O(N)$$.  The analysis is the same as *Approach #1*.

---
#### Approach #3: Regular Expressions [Accepted]

**Intuition and Algorithm**

Whenever parsing is involved, we can use *regular expressions*, a language for defining patterns in text.

Our regular expression will be `"([A-Z][a-z]*)(\d*)|(\()|(\))(\d*)"`.  Breaking this down by *capture group*, this is:

* `([A-Z][a-z]*)` Match an uppercase character followed by any number of lowercase characters, then (`(\d*)`) match any number of digits.
* OR, `(\()` match a left bracket or `(\))` right bracket, then `(\d*)` match any number of digits.

Now we can proceed as in *Approach #2*.

* If we parsed a name and multiplicity `([A-Z][a-z]*)(\d*)`, we will add it to our current count.

* If we parsed a left bracket, we will append a new `count` to our stack, representing the nested depth of parentheses.

* If we parsed a right bracket (and possibly another multiplicity), we will multiply our deepest level `count`, `top = stack.pop()`, and add those entries to our current count.


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

* Time Complexity $$O(N^2)$$, and Space Complexity $$O(N)$$.  The analysis is the same as *Approach #1*, as this regular expression did not look backwards when parsing.

---

Analysis written by: [@awice](https://leetcode.com/awice).  Approaches #1 and #2 inspired by [@zestypanda](https://leetcode.com/zestypanda/).  Java solution for #3 by [@jianchao.li.fighter](https://discuss.leetcode.com/user/jianchao-li-fighter).
