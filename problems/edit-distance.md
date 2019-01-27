

## Solution

---

#### Intuition

The edit distance algorithm is very popular among 
the data scientists. It's one of the basic algorithms
used for evaluation of machine translation and speech 
recognition. 

The naive approach would be to check for all possible edit 
sequences and choose the shortest one in-between.
That would result in an exponential complexity and it's an overkill
since we actually don't need to have all possible edit sequences 
but just the shortest one. 

#### Approach 1: Dynamic Programming

The idea would be to reduce the problem to simple ones.
For example, there are two words, `horse` and `ros` and we want to compute
an edit distance `D` for them. One could notice that it seems to be
more simple for short words and so it would be logical to relate
an edit distance `D[n][m]` with the lengths `n` and `m` of input words.

Let's go further and introduce an edit distance `D[i][j]` which is
an edit distance between the first `i` characters of `word1` and 
the first `j` characters of `word2`.

![edit_distance](../Figures/72/72_edit.png)

It turns out that one could compute `D[i][j]`, knowing 
`D[i - 1][j]`, `D[i][j - 1]` and `D[i - 1][j - 1]`.

> There is just one more character to add into one or both strings 
and the formula is quite obvious.

If the last character is the same, *i.e.* `word1[i] = word2[j]` then

$$
D[i][j] = 1 + \min(D[i - 1][j], D[i][j - 1], D[i - 1][j - 1] - 1)
$$

and if not, *i.e.* `word1[i] != word2[j]` we have to
take into account the replacement of the last character 
during the conversion.

$$
D[i][j] = 1 + \min(D[i - 1][j], D[i][j - 1], D[i - 1][j - 1])
$$

So each step of the computation would be done based on the previous computation,
as follows: 

![compute](../Figures/72/72_compute.png)

The obvious base case is an edit distance between the empty string and 
non-empty string that means `D[i][0] = i` and `D[0][j] = j`.

Now we have everything to actually proceed to the computations 

<!--![LIS](../Figures/72/72_tr.gif)-->
!?!../Documents/72_LIS.json:1000,513!?!


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

* Time complexity : $$\mathcal{O}(m n)$$ as 
it follows quite straightforward for the inserted loops. 
* Space complexity : $$\mathcal{O}(m n)$$ since at each step we
keep the results of all previous computations.

Analysis written by @[liaison](https://leetcode.com/liaison/)
and @[andvary](https://leetcode.com/andvary/)
