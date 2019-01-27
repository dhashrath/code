

#### Approach #1: Recursive Parsing [Accepted]

**Intuition and Algorithm**

This question is relatively straightforward in terms of the idea of the solution, but presents substantial difficulties in the implementation.

Expressions may involve the evaluation of other expressions, which motivates a recursive approach.

One difficulty is managing the correct scope of the variables.  We can use a stack of hashmaps.  As we enter an inner scope defined by parentheses, we need to add that scope to our stack, and when we exit, we need to pop that scope off.

Our main `evaluate` function will go through each case of what form the `expression` could take.

* If the expression starts with a digit or '-', it's an integer: return it.

* If the expression starts with a letter, it's a variable.  Recall it by checking the current scope in reverse order.

* Otherwise, group the tokens (variables or expressions) within this expression by counting the "balance" `bal` of the occurrences of `'('` minus the number of occurrences of `')'`.  When the balance is zero, we have ended a token.  For example, `(add 1 (add 2 3))` should have tokens `'1'` and `'(add 2 3)'`.

* For add and mult expressions, evaluate each token and return the addition or multiplication of them.

* For let expressions, evaluate each expression sequentially and assign it to the variable in the current scope, then return the evaluation of the final expression.


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

* Time Complexity: $$O(N^2)$$, where $$N$$ is the length of `expression`.  Each expression is evaluated once, but within that evaluation we may search the entire scope.

* Space Complexity: $$O(N^2)$$.  We may pass $$O(N)$$ new strings to our `evaluate` function when making intermediate evaluations, each of length $$O(N)$$.  With effort, we could reduce the total space complexity to $$O(N)$$ with interning or passing pointers.

---

Analysis written by: [@awice](https://leetcode.com/awice).
