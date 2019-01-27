

---
#### Approach #1: Next Array [Accepted]

**Intuition**

Let `left[i]` be the distance from seat `i` to the closest person sitting to the left of `i`.  Similarly, let `right[i]` be the distance to the closest person sitting to the right of `i`.  This is motivated by the idea that the closest person in seat `i` sits a distance `min(left[i], right[i])` away.

**Algorithm**

To construct `left[i]`, notice it is either `left[i-1] + 1` if the seat is empty, or `0` if it is full.  `right[i]` is constructed in a similar way.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `seats`.

* Space Complexity:  $$O(N)$$, the space used by `left` and `right`.


---
#### Approach #2: Two Pointer [Accepted]

**Intuition**

As we iterate through seats, we'll update the closest person sitting to our left, and closest person sitting to our right.

**Algorithm**

Keep track of `prev`, the filled seat at or to the left of `i`, and `future`, the filled seat at or to the right of `i`.

Then at seat `i`, the closest person is `min(i - prev, future - i)`, with one exception.  `i - prev` should be considered infinite if there is no person to the left of seat `i`, and similarly `future - i` is infinite if there is no one to the right of seat `i`.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `seats`.

* Space Complexity:  $$O(1)$$.



---
#### Approach #3: Group by Zero [Accepted]

**Intuition**

In a group of `K` adjacent empty seats between two people, the answer is `(K+1) / 2`.

**Algorithm**

For each group of `K` empty seats between two people, we can take into account the candidate answer `(K+1) / 2`.

For groups of empty seats between the edge of the row and one other person, the answer is `K`, and we should take into account those answers too.


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

* Time Complexity:  $$O(N)$$, where $$N$$ is the length of `seats`.

* Space Complexity:  $$O(1)$$.  (In Python, `seats[::-1]` uses $$O(N)$$ space, but this can be remedied.)


---

Analysis written by: [@awice](https://leetcode.com/awice).
