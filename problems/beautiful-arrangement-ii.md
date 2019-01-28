#### Beautiful Arrangement II

```java
class Solution {
    private ArrayList<ArrayList<Integer>> permutations(int[] nums) {
        ArrayList<ArrayList<Integer>> ans = new ArrayList<ArrayList<Integer>>();
        permute(ans, nums, 0);
        return ans;
    }

    private void permute(ArrayList<ArrayList<Integer>> ans, int[] nums, int start) {
        if (start >= nums.length) {
            ArrayList<Integer> cur = new ArrayList<Integer>();
            for (int x : nums) cur.add(x);
            ans.add(cur);
        } else {
            for (int i = start; i < nums.length; i++) {
                swap(nums, start, i);
                permute(ans, nums, start+1);
                swap(nums, start, i);
            }
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }

    private int numUniqueDiffs(ArrayList<Integer> arr) {
        boolean[] seen = new boolean[arr.size()];
        int ans = 0;

        for (int i = 0; i < arr.size() - 1; i++) {
            int delta = Math.abs(arr.get(i) - arr.get(i+1));
            if (!seen[delta]) {
                ans++;
                seen[delta] = true;
            }
        }
        return ans;
    }

    public int[] constructArray(int n, int k) {
        int[] nums = new int[n];
        for (int i = 0; i < n; i++) {
            nums[i] = i+1;
        }
        for (ArrayList<Integer> cand : permutations(nums)) {
            if (numUniqueDiffs(cand) == k) {
                int[] ans = new int[n];
                int i = 0;
                for (int x : cand) ans[i++] = x;
                return ans;
            }
        }
        return null;
    }
}```


```python
class Solution(object):
    def constructArray(self, n, k):
        seen = [False] * n
        def num_uniq_diffs(arr):
            ans = 0
            for i in range(n):
                seen[i] = False
            for i in range(len(arr) - 1):
                delta = abs(arr[i] - arr[i+1])
                if not seen[delta]:
                    ans += 1
                    seen[delta] = True
            return ans

        for cand in itertools.permutations(range(1, n+1)):
            if num_uniq_diffs(cand) == k:
                return cand```


```java
class Solution {
    public int[] constructArray(int n, int k) {
        int[] ans = new int[n];
        int c = 0;
        for (int v = 1; v < n-k; v++) {
            ans[c++] = v;
        }
        for (int i = 0; i <= k; i++) {
            ans[c++] = (i%2 == 0) ? (n-k + i/2) : (n - i/2);
        }
        return ans;
    }
}```


```python
class Solution(object):
    def constructArray(self, n, k):
        ans = list(range(1, n - k))
        for i in range(k+1):
            if i % 2 == 0:
                ans.append(n-k + i//2)
            else:
                ans.append(n - i//2)

        return ans```

