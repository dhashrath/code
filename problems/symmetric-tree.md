## Solution
---

#### Approach 1: Recursive

A tree is symmetric if the left subtree is a mirror reflection of the right subtree.

![Push an element in stack](https://leetcode.com/media/original_images/101_Symmetric.png){:width="200px"}
{:align="center"}

Therefore, the question is: when are two trees a mirror reflection of each other?

Two trees are a mirror reflection of each other if:

1. Their two roots have the same value.
2. The right subtree of each tree is a mirror reflection of the left subtree of the other tree.

![Push an element in stack](https://leetcode.com/media/original_images/101_Symmetric_Mirror.png){:width="400px"}
{:align="center"}

This is like a person looking at a mirror. The reflection in the mirror has the same head, but the reflection's right arm corresponds to the actual person's left arm, and vice versa.

The explanation above translates naturally to a recursive function as follows.


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

* Time complexity : $$O(n)$$. Because we traverse the entire input tree once, the total run time is $$O(n)$$, where $$n$$ is the total number of nodes in the tree.


* Space complexity : The number of recursive calls is bound by the height of the tree. In the worst case, the tree is linear and the height is in $$O(n)$$. Therefore, space complexity due to recursive calls on the stack is $$O(n)$$ in the worst case.
<br />
<br />
---
#### Approach 2: Iterative

Instead of recursion, we can also use iteration with the aid of a queue. Each two consecutive nodes in the queue should be equal, and their subtrees a mirror of each other. Initially, the queue contains `````root````` and `````root`````. Then the algorithm works similarly to BFS, with some key differences. Each time, two nodes are extracted and their values compared. Then, the right and left children of the two nodes are inserted in the queue in opposite order. The algorithm is done when either the queue is empty, or we detect that the tree is not symmetric (i.e. we pull out two consecutive nodes from the queue that are unequal).


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

* Time complexity : $$O(n)$$. Because we traverse the entire input tree once, the total run time is $$O(n)$$, where $$n$$ is the total number of nodes in the tree.


* Space complexity : There is additional space required for the search queue. In the worst case, we have to insert $$O(n)$$ nodes in the queue. Therefore, space complexity is $$O(n)$$.
