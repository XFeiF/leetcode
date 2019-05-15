[867. Transpose Matrix](https://leetcode.com/problems/transpose-matrix/)  

### Description  

Given a matrix `A`, return the transpose of `A`.

The transpose of a matrix is the matrix flipped over it's main diagonal, switching the row and column indices of the matrix.  

### Explaination

第一版代码：  

```python
class Solution:
    def transpose(self, A):
        """
        :type A: List[List[int]]
        :rtype: List[List[int]]
        """
        r = len(A)
        c = len(A[0])
        res = []
        for col in range(c):
            cur = []
            for row in A:
                cur.append(row[col])
            res.append(cur)
        return res
```



第二版代码：  

```python
return [[row[col] for row in A]  for col in range(len(A[0]))]
```



Lee的代码：  

```python
return list(zip(*A))
```

