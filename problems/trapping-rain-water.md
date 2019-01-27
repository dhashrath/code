

## Solution
---
#### Approach 1: Brute force

**Intuition**

Do as directed in question. For each element in the array, we find the maximum level of water it can trap after the rain, which is equal to the minimum of maximum height of bars on both the sides minus its own height.

**Algorithm**

* Initialize $$ans=0$$
* Iterate the array from left to right:
	+ Initialize $$\text{max_left}=0$$ and $$\text{max_right}=0$$
	+ Iterate from the current element to the beginning of array updating:
		* $$\text{max_left}=\max(\text{max_left},\text{height}[j])$$
	+ Iterate from the current element to the end of array updating:
		* $$\text{max_right}=\max(\text{max_right},\text{height}[j])$$
	+ Add $$\min(\text{max_left},\text{max_right}) - \text{height}[i]$$ to $$\text{ans}$$


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

* Time complexity: $$O(n^2)$$. For each element of array, we iterate the left and right parts.

* Space complexity: $$O(1)$$ extra space.
<br />
<br />
---
#### Approach 2: Dynamic Programming

**Intuition**

In brute force, we iterate over the left and right parts again and again just to find the highest bar size upto that index. But, this could be stored. Voila, dynamic programming.

The concept is illustrated as shown:

![Dynamic programming](../Figures/42/trapping_rain_water.png){:width="500px"}
{:align="center"}

**Algorithm**

* Find maximum height of bar from the left end upto an index i in the array $$\text{left_max}$$.
* Find maximum height of bar from the right end upto an index i in the array $$\text{right_max}$$.
* Iterate over the $$\text{height}$$ array and update ans:
	+ Add $$\min(\text{max_left}[i],\text{max_right}[i]) - \text{height}[i]$$ to $$ans$$


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

* Time complexity: $$O(n)$$.
	+ We store the maximum heights upto a point using 2 iterations of $$O(n)$$ each.
	+ We finally update $$\text{ans}$$ using the stored values in $$O(n)$$.

* Space complexity: $$O(n)$$ extra space.
	+ Additional $$O(n)$$ space for $$\text{left_max}$$ and $$\text{right_max}$$ arrays than in [Approach 1](#approach-1-brute-force).
<br />
<br />
---
#### Approach 3: Using stacks

**Intuition**

Instead of storing the largest bar upto an index as in [Approach 2](#approach-2-dynamic-programming), we can use stack to keep track of the bars that are bounded by longer bars and hence, may store water. Using the stack, we can do the calculations in only one iteration.

We keep a stack and iterate over the array. We add the index of the bar to the stack if bar is smaller than or equal to the bar at top of stack, which means that the current bar is bounded by the previous bar in the stack. If we found a bar longer than that at the top, we are sure that the bar at the top of the stack is bounded by the current bar and a previous bar in the stack, hence, we can pop it and add resulting trapped water to $$\text{ans}$$.

**Algorithm**

* Use stack to store the indices of the bars.
* Iterate the array:
	+ While stack is not empty and $$\text{height}[current]>\text{height}[st.top()]$$
		* It means that the stack element can be popped. Pop the top element as $$\text{top}$$.
		* Find the distance between the current element and the element at top of stack, which is to be filled.
		$$\text{distance} = \text{current} - \text{st.top}() - 1$$
		* Find the bounded height
		$$\text{bounded_height} = \min(\text{height[current]}, \text{height[st.top()]}) - \text{height[top]}$$
		* Add resulting trapped water to answer
    $$\text{ans} += \text{distance} * \text{bounded_height}$$
	+ Push current index to top of the stack
	+ Move $$\text{current}$$ to the next position



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

* Time complexity: $$O(n)$$.
	+ Single iteration of $$O(n)$$ in which each bar can be touched at most twice(due to  insertion and deletion from stack) and insertion and deletion from stack takes $$O(1)$$ time.
* Space complexity: $$O(n)$$. Stack can take upto $$O(n)$$ space in case of stairs-like or flat structure.
<br />
<br />
---
#### Approach 4: Using 2 pointers

**Intuition**
As in [Approach 2](#approach-2-dynamic-programming), instead of computing the left and right parts seperately, we may think of some way to do it in one iteration.
From the figure in dynamic programming approach, notice that as long as $$\text{right_max}[i]>\text{left_max}[i]$$ (from element 0 to 6), the water trapped depends upon the left_max, and similar is the case when $$\text{left_max}[i]>\text{right_max}[i]$$ (from element 8 to 11).
So, we can say that if there is a larger bar at one end (say right), we are assured that the water trapped would be dependant on height of bar in current direction (from left to right). As soon as we find the bar at other end (right) is smaller, we start iterating in opposite direction (from right to left).
We must maintain $$\text{left_max}$$ and $$\text{right_max}$$ during the iteration, but now we can do it in one iteration using 2 pointers, switching between the two.

**Algorithm**

* Initialize $$\text{left}$$ pointer to 0 and $$\text{right}$$ pointer to size-1
* While $$\text{left}< \text{right}$$, do:
	+ If $$\text{height[left]}$$ is smaller than $$\text{height[right]}$$
		* If $$\text{height[left]}>=\text{left_max}$$, update $$\text{left_max}$$
		* Else add $$\text{left_max}-\text{height[left]}$$ to $$\text{ans}$$
		* Add 1 to $$\text{left}$$.
	+ Else
		* If $$\text{height[right]}>=\text{right_max}$$, update $$\text{right_max}$$
		* Else add $$\text{right_max}-\text{height[right]}$$ to $$\text{ans}$$
		* Subtract 1 from $$\text{right}$$.

Refer the example for better understanding:
!?!../Documents/42_trapping_rain_water.json:1000,662!?!


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

* Time complexity: $$O(n)$$. Single iteration of $$O(n)$$.
* Space complexity: $$O(1)$$ extra space. Only constant space required for $$\text{left}$$, $$\text{right}$$, $$\text{left_max}$$ and $$\text{right_max}$$.
