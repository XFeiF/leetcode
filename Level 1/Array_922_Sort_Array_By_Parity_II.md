[922. Sort Array By Parity II](https://leetcode.com/problems/sort-array-by-parity-ii/)

```python

class Solution:
    def sortArrayByParityII(self, A):
        """
        :type A: List[int]
        :rtype: List[int]
        """
        odd = []
        even = []
        for x in A:
            if x % 2:
                even.append(x)
            else:
                odd.append(x)
        res = []
        for i in range(len(odd)):
            res.append(odd[i])
            res.append(even[i])
        return res
```

one pass  

```python
class Solution:
    def sortArrayByParityII(self, A):
        """
        :type A: List[int]
        :rtype: List[int]
        """
        even = 0
        odd  = 1
        sz = len(A)
        while even < sz and odd < sz:
            if A[even] % 2 == 0:
                even += 2
            elif A[odd] % 2:
                odd += 2
            else:
                A[even], A[odd] = A[odd], A[even]
                even += 2
                odd += 2
        return A
```

