

## Solution
---

#### Approach 1: Backtracking

**Intuition**

Let us first look at what the question asks us to do before getting at the approach to solve it. So, we are given a string of numbers and 3 different operators:

* `+` Addition,
* `-` Subtraction or
* `*` Multiplication

We have to find all possible combinations of binary operators between the digits so that the overall value of the resulting expression becomes equal to a given target value. Let us look at a few possibilities of what it means exactly to *place the operators between digits* so that the question becomes clearer.

Let's say we are given the following set of digits `"123456789"` and the target value given to us is `45`. Let us see some of the possible resulting expressions that we can get by placing the operators in different locations.

<pre>
1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 = 45
1 + 2 - 3 + 4 - 5 + 6 - 7 + 8 - 9 = -3
1 + 2 * 3 - 4 + 5 + 6 - 7 * 8 - 9 = -51
1 + 2 + 3 + 4 + 5 - 6 * 7 + 8 * 9 = 45
</pre>

These are just 4 of the many resulting expressions that are possible by using the given string of digits and the three operators.

By looking at the above examples we can't really figure out any specific pattern among the resulting expressions that tells us which of them will give us the resulting target.

Since the question explicitly states that we are given binary operators, this means that each of the operator would require two operands.

> We can consider each of our digits as an operand.

This means that between every pair of digits we can have any of the three operators i.e. $$+$$, $$-$$ or $$\times$$.

If you've looked at the question's statement and the examples that are given in the question, you would realize that there is an example where the digits are `"105"` and the target value is `5`. For this particular example, there are two expressions given to us and they are `1*0+5` and `10-5`.

The second expression is something that you need to look out for before getting to solve this question because this complicates things a bit.

It would have been an easier question to solve if we just had to consider those expressions that simply had *digits as operands*.

But, in this question, we can have all sorts of digits getting together and forming a bigger number that becomes a part of the expression. Let us look at some example expressions for the digits `"123456"` and target `30`.

<pre>
1 * 23 - 4 + 5 + 6 = 30
12 - 3 * 4 + 5 * 6 = 30
1 - 23 - 4 + 56 = 30
</pre>

So this means that although the number of operators that we have are defined for us i.e. 3 different binary operators, but the number of operands are **not really well defined for us**.

This is a big portion of the original problem that we need to address in our solution.

Since we are asked to find out all of the valid expressions whose value equals the given target and we don't really know what specific operator between two operands would eventually give us a valid expression,

> We try out all of the options.

This means that once we have defined what the operands are for our given expression, we would have three possible choices of operators between each consecutive pair of operands.

From an implementation perspective, what would an operand imply with respect to our original string?

> An operand would be an integer formed from a substring of our original string.

Let's look at two different array partitions for the given string `"123456789"`

<center>
<img src="../Figures/282/282_Expression_Add_Operators_Diag_1.png" height="300"></center>

Since we are required to return all of the valid expressions that evaluate to a given target value, we have to try all possible partitions of the given array thereby considering all of the possible operands that can be formed from the digits.

This `try out everything` hints at a backtracking solution and that is exactly what we are going to look at here.

**Algorithm**

Let's quickly look at the steps involved in our backtracking algorithm before looking at the pseudo-code.

1. As discussed above, we have multiple choices of what operators to use and what the operands can be and hence, we have to look at all the possibilities to find ***all*** valid expressions.
2. Our recursion step will have an `index` and the original string of digits as input along with the expression built till now.
3. At every step, we will consider all possible operands starting from `index` i.e. all substrings $$digits[index : ]$$ and once we fix an operand, we will have three recursive calls wherein we try 3 different operators after this operand.
4. We keep on building our expression like this and eventually, the entire string would be processed. At that time we check if the expression we built till now is a valid expression or not and we record it if it is a valid one.

<pre>
1. procedure recurse(digits, index, expression):
2.     if we have reached the end of the string:
3.         if the expression evaluates to the target:
4.             Valid Expression found!
5.     else:
6.         for all operands starting from current index:
7.             try out operator * and recurse
8.             try out operator + and recurse
9.             try out operator - and recurse
</pre>

The algorithm now looks pretty straightforward. However, the implementation is something that needs more thought and there are some things that we need to address before actually looking at the implementation.

When we are done building an expression out of all of the digits in our original string i.e. the base case, then we check if the expression is a valid expression or not. Right ?

> How do we actually check if an expression is a valid one or not if all we have is a string representing the expression and not the integer value for the same?

Well, one way to go about this is to write a custom `eval` function that takes in a string and returns the value of that expression. If you do that (Python people can use the inbuilt function `eval` for this), you will get a TLE i.e. time limit exceeded error.

<br/>

**Can't we keep track of the expression's value on the fly?**

Well yes. That's the idea we will go with. Instead of just keeping track of what the expression string is, we will also keep track of it's value along the way so that when the recursion hits the base case, we can check in $$O(1)$$ time if the expression's value equals the target value or not.

The implementation would have been straightforward had it just been `+` and `-` operators involved. This is because both these operators have an equal precedence. That means that we can continue to evaluate the expression on the fly without any problems. Have a look at the following example.

<center>
<img src="../Figures/282/282_Expression_Add_Operators_Diag_2.png" width="550"></center>

So far so good. Now let us add the `*` operator as well and see how building the expression on the fly like this breaks.

<center>
<img src="../Figures/282/282_Expression_Add_Operators_Diag_3.png" width="550"></center>

What we mean by building the expression on the fly is that we keep track of the expression's value till now and we simply consider that value as one of the two operands for our operators. As we can see from the two examples above, this would have worked had it just been `+` and `-` operators.

But, this approach is bound to fail because the `*` operator takes precedence over `+` and `-`. The `*` operator would require the ***actual*** previous operand in our expression rather than the current value of the expression. i.e. In the above example, the `*` operator needed `2` rather than `12` to get us the correct value of `18`.

<br/>

**How to handle this?**

The idea on how to handle this problem springs from the discussion above. We simply need to keep track of the last operand in our expression and how it modified the expression's value overall so that when we consider the `*` operator, we can **reverse** the effects of the previous operand and consider it for multiplication. Let's take a look at the example that was breaking before.

<center>
<img src="../Figures/282/282_Expression_Add_Operators_Diag_4.png" width="550"></center>

Now we can look at the actual implementation of this algorithm.


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

For the complexity analysis, let us look at what the major sections of the code are.

1. The first is considering all of the operands. Operands essentially is a substring of the original string. There are $$O(N^2)$$ possible substrings if the string is of length N.
2. Then, for every operand we have $$3$$ different choices of operators that can come after it. These three operators translate to 3 different recursive calls.
3. Another one that doesn't seem like much but will come into the picture for Python and Java is keeping track of the expression till now. i.e. the `ops` variable in the code above.
4. Finally, the complexity of handling the base case.


**Time Complexity**

* Time complexity for considering all operators for all operands, $$O(3^N)$$. This is $$O(3^N)$$ because in the worst case scenario, each digit will be an operand on its own for a single expression and between every adjacent pair of digits we have 3 different choices for operators.
* Also, for each recursive call we have a `for` loop to consider successive digits as a single operand. That raises the total number of recursive calls to $$O(N \times 3^N)$$
* For the base case we use a `StringBuilder::toString` operation in Java and `.join()` operation in Python and that takes $$O(N)$$ time. Here N represents the length of our expression. In the worst case, each digit would be an operand and we would have $$N$$ digits and $$N - 1$$ operators. So $$O(N)$$. This is for one expression. In the worst case, we can have $$O(N^2 \times 3^N)$$ valid expressions.
* Overall time complexity = $$O(N^2 \times 3^N)$$.

**Space Complexity** :

* The answers array that holds all of our expressions is something that is common in both the implementations. The space occupied is equivalent to the number of valid expressions which in the worst case be all of the expressions. Hence, the size of the answer array would essentially be equal to the total recursive calls which as stated before are $$O(N^2 \times 3^N)$$.
* An important thing that we should not ignore about the space complexity is the intermediate data structures we are using in both Python and Java. For every recursive call we create a new `StringBuffer` in Java or a new `list` in Python and there will be $$O(N^2 \times 3^N)$$ recursive calls.
* Overall space complexity = $$O(N^2 \times 3^N)$$.

<br />
<br />


---


Analysis written by: [@sachinmalhotra1993](https://leetcode.com/sachinmalhotra1993).
