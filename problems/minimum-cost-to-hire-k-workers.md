

## Solution
---
#### Approach 1: Greedy

**Intuition**

At least one worker will be paid their minimum wage expectation.  If not, we could scale all payments down by some factor and still keep everyone earning more than their wage expectation.

**Algorithm**

For each `captain` worker that will be paid their minimum wage expectation, let's calculate the cost of hiring K workers where each point of quality is worth `wage[captain] / quality[captain]` dollars.  With this approach, the remaining implementation is straightforward.

Note that this algorithm would not be efficient enough to pass larger test cases.


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

* Time Complexity:  $$O(N^2 \log N)$$, where $$N$$ is the number of workers.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---
#### Approach 2: Heap

**Intuition**

As in *Approach #1*, at least one worker is paid their minimum wage expectation.

Additionally, every worker has some minimum `ratio` of dollars to quality that they demand.  For example, if `wage[0] = 100` and `quality[0] = 20`, then the `ratio` for worker 0 is `5.0`.

The key insight is to iterate over the ratio.  Let's say we hire workers with a ratio `R` or lower.  Then, we would want to know the `K` workers with the lowest quality, and the sum of that quality.  We can use a heap to maintain these variables.

**Algorithm**

Maintain a max heap of quality.  (We're using a minheap, with negative values.)  We'll also maintain `sumq`, the sum of this heap.

For each worker in order of ratio, we know all currently considered workers have lower ratio.  (This worker will be the 'captain', as described in *Approach #1*.)  We calculate the candidate answer as this ratio times the sum of the smallest K workers in quality.


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

* Time Complexity:  $$O(N \log N)$$, where $$N$$ is the number of workers.

* Space Complexity:  $$O(N)$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
