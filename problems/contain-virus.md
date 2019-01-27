

#### Approach #1: Simulation [Accepted]

**Intuition**

Let's work on simulating one turn of the process.  We can repeat this as necessary while there are still infected regions.

**Algorithm**

Though the implementation is long, the algorithm is straightforward.  We perform the following steps:

* Find all viral regions (connected components), additionally for each region keeping track of the frontier (neighboring uncontaminated cells), and the perimeter of the region.

* Disinfect the most viral region, adding it's perimeter to the answer.

* Spread the virus in the remaining regions outward by 1 square.


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

* Time Complexity: $$O((R*C)^{\frac{4}{3}})$$ where $$R, C$$ is the number of rows and columns.  After time $$t$$, viral regions that are alive must have size at least $$t^2 + (t-1)^2$$, so the total number removed across all time is $$\Omega(t^3) \leq R*C$$.

* Space Complexity: $$O(R*C)$$ in additional space.

---

Analysis written by: [@awice](https://leetcode.com/awice).
