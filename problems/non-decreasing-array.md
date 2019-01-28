#### Non-decreasing Array

```python
class Solution(object):
    def checkPossibility(self, A):
        def monotone_increasing(arr):
            for i in range(len(arr) - 1):
                if arr[i] > arr[i+1]:
                    return False
            return True

        new = A[:]
        for i in xrange(len(A)):
            old_ai = A[i]
            new[i] = new[i-1] if i > 0 else float('-inf')
            if monotone_increasing(new):
                return True
            new[i] = old_ai

        return False```


```python
class Solution(object):
    def checkPossibility(self, A):
        def brute_force(A):
            #Same as in approach 1

        i, j = 0, len(A) - 1
        while i+2 < len(A) and A[i] <= A[i+1] <= A[i+2]:
            i += 1
        while j-2 >= 0 and A[j-2] <= A[j-1] <= A[j]:
            j -= 1

        if j - i + 1 <= 2:
            return True
        if j - i + 1 >= 5:
            return False

        return brute_force(A[i: j+1])```


```python
class Solution(object):
    def checkPossibility(self, A):
        p = None
        for i in xrange(len(A) - 1):
            if A[i] > A[i+1]:
                if p is not None:
                    return False
                p = i

        return (p is None or p == 0 or p == len(A)-2 or
                A[p-1] <= A[p+1] or A[p] <= A[p+2])```

