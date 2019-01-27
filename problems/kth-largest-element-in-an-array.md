

## Solution

---

#### Approach 0: Sort

The naive solution would be to sort an array first and then return kth 
element from the end, something like `sorted(nums)[-k]` on Python. 
That would be an algorithm of $$\mathcal{O}(N \log(N))$$ time complexity
and $$\mathcal{O}(1)$$ space complexity.
This time complexity is not really exciting so 
let's check how to improve it by using some additional space.
<br />
<br />


---     
#### Approach 1: Heap

The idea is to init a heap "the smallest element first",
and add all elements from the array into this heap one by one
keeping the size of the heap always less or equal to `k`. 
That would results in a heap containing `k` largest elements of the array.

The head of this heap is the answer, 
i.e. the kth largest element of the array. 

The time complexity of adding an element in a heap of size `k`
is $$\mathcal{O}(\log(k))$$, and we do it `N` times that means 
$$\mathcal{O}(N \log(k))$$ time complexity for the algorithm.

In Python there is a method `nlargest` in `heapq` library 
which has the same $$\mathcal{O}(N \log(k))$$
time complexity and reduces the code to one line.

This algorithm improves time complexity, but one pays with 
$$\mathcal{O}(k)$$ space complexity.
        
<!--![LIS](../Figures/72/72_tr.gif)-->
!?!../Documents/215_LIS.json:1000,530!?!


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

* Time complexity : $$\mathcal{O}(N \log(k))$$. 
* Space complexity : $$\mathcal{O}(k)$$ to store the heap elements. 
<br />
<br />


---
#### Approach 2: Quickselect 

This [textbook algorthm](https://en.wikipedia.org/wiki/Quickselect) 
has $$\mathcal{O}(N)$$ average time complexity.
Like quicksort, it was developed by Tony Hoare, 
and is also known as _Hoare's selection algorithm_.

The approach is basically the same as for quicksort. 
For simplicity let's notice that `k`th largest element is the same as
`N - k`th smallest element, hence one could implement 
`k`th smallest algorithm for this problem.

First one chooses a pivot, 
and defines its position in a sorted array in a linear time.
This could be done with the help of _partition algorithm_.

> To implement partition one moves along an array,
compares each element with a pivot, 
and moves all elements smaller than pivot to the left of the pivot.
 
As an output we have an array where pivot is on its perfect position
in the ascending sorted array, 
all elements on the left of the pivot are smaller than pivot,
and all elements on the right of the pivot are larger or equal to pivot.

Hence the array is now split into two parts.
If that would be a quicksort algorithm, one would proceed recursively 
to use quicksort for the both parts that would result in 
$$\mathcal{O}(N \log(N))$$ time complexity.
Here there is no need to deal with both parts since now one knows 
in which part to search for `N - k`th smallest element, and that
reduces average time complexity to $$\mathcal{O}(N)$$.

Finally the overall algorithm is quite straightforward :

* Choose a random pivot.

* Use a partition algorithm to place the pivot 
into its perfect position `pos` in the sorted array,
move smaller elements to the left of pivot, and larger or equal ones - to the right.

* Compare `pos` and `N - k` to choose the side of array to proceed recursively.

> ! Please notice that this algorithm works well even for arrays with duplicates.

![quickselect](../Figures/215/215_quickselect.png)


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


* Time complexity : $$\mathcal{O}(N)$$ in the average case, $$\mathcal{O}(N^2)$$ in the worst case. 
* Space complexity : $$\mathcal{O}(1)$$. 


Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
