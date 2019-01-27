

## Solution

**Tree definition**

First of all, here is the definition of the ```TreeNode``` which we would use.


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

<br />
<br />


---
#### Approach 1: Recursion

First of all let's count how many trees do we have to construct.
As you could check in [this article](https://leetcode.com/articles/unique-binary-search-trees/),
the number of possible BST is actually a [Catalan number](https://en.wikipedia.org/wiki/Catalan_number). 

Let's follow the logic from the above article, this time not to count but
to actually construct the trees. 
 
**Algorithm**

Let's pick up number `i` out of the sequence `1 ..n` and use it as the root
of the current tree. 
Then there are ```i - 1``` elements available for the construction of the
left subtree and ```n - i``` elements available for the right subtree. 
As we [already discussed](https://leetcode.com/articles/unique-binary-search-trees/)
that results in ```G(i - 1)``` different left subtrees and ```G(n - i)```
different right subtrees, where ```G``` is a Catalan number. 

![BST](../Figures/96_BST.png)

Now let's repeat the step above for the sequence `1 ... i - 1` to construct
all left subtrees, and then for the sequence `i + 1 ... n` to construct all
right subtrees. 

This way we have a root `i` and two lists for the possible left and right subtrees.
The final step is to loop over both lists to link left and right
subtrees to the root.  


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


**Complexity analysis**

* Time complexity : The main computations are to construct all possible 
trees with a given root, that is actually Catalan number $$G_n$$ as was discussed
above. This is done `n` times, that results in time complexity $$n G_n$$.
Catalan numbers grow as $$\frac{4^n}{n^{3/2}}$$ that gives the 
final complexity $$\mathcal{O}(\frac{4^n}{n^{1/2}})$$.
Seems to be large but let's not forget that here we're asked to generate
$$G_n \sim \frac{4^n}{n^{3/2}}$$ tree objects as output.

* Space complexity : $$n G_n$$ as we keep $$G_n$$ trees with `n` elements each, 
that results in $$\mathcal{O}(\frac{4^n}{n^{1/2}})$$
complexity.  
 
Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
