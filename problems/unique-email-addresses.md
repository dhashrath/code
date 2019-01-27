

## Solution
---
#### Approach 1: Canonical Form

**Intuition and Algorithm**

For each email address, convert it to the *canonical* address that actually receives the mail.  This involves a few steps:

* Separate the email address into a `local` part and the `rest` of the address.

* If the `local` part has a `'+'` character, remove it and everything beyond it from the `local` part.

* Remove all the zeros from the `local` part.

* The canonical address is `local + rest`.

After, we can count the number of unique canonical addresses with a `Set` structure.


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

* Time Complexity:  $$O(\mathcal{C})$$, where $$\mathcal{C}$$ is the total content of `emails`.

* Space Complexity:  $$O(\mathcal{C})$$.
<br />
<br />


---


Analysis written by: [@awice](https://leetcode.com/awice).
