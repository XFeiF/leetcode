[766. Toeplitz Matrix](https://leetcode.com/problems/toeplitz-matrix/)  

### Description  

A matrix is *Toeplitz* if every diagonal from top-left to bottom-right has the same element.

Now given an `M x N` matrix, return `True` if and only if the matrix is *Toeplitz*.

**Example 1:**

```
Input:
matrix = [
  [1,2,3,4],
  [5,1,2,3],
  [9,5,1,2]
]
Output: True
Explanation:
In the above grid, the diagonals are:
"[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]".
In each diagonal all elements are the same, so the answer is True.

```

**Example 2:**

```
Input:
matrix = [
  [1,2],
  [2,2]
]
Output: False
Explanation:
The diagonal "[1, 2]" has different elements.

```

**Note:**

1. `matrix` will be a 2D array of integers.
2. `matrix` will have a number of rows and columns in range `[1, 20]`.
3. `matrix[i][j]` will be integers in range `[0, 99]`.



### Solution

**First Commit:**

```python
class Solution:
    def isToeplitzMatrix(self, m):
        """
        :type matrix: List[List[int]]
        :rtype: bool
        """
        for i in range(len(m) - 1):
            for j in range(len(m[0]) - 1):
                if m[i][j] != m[i + 1][j + 1]:
                    return False
        return True
```

**Shorten:**

```python
return all(m[i][j] != m[i+1][j+1] for i in range(len(m)-1) for j in range(len(m[0])-1))
```

**pythonic：**

这里强推Lee的第三种`shorter and more pythonic`的方法。  

先补充一下`zip`的用法：  

> The `zip()` function returns a zip object, which is an iterator of tuples where the first item in each passed iterator is paired together, and then the second item in each passed iterator are paired together etc. 
>
> If the passed iterators have different lengths, the iterator with least items decides the length of the new iterator.

就是说`zip`函数接收若干个迭代器，迭代器长度取决于其中最短的那个。之后接受一个进行配对，取这些迭代器中相同位置的元素组成新的tuple对象。  

这里我们的两个循环其实就是想实现第一行和第二行匹配、第二行和第三行匹配、… 、倒数第二行和倒数第一行匹配，那么总结起来就是`zip(m, m[1:])`，因为取短的那个，所以取`len(m)-1`个，就是从第1个到倒数第二个。  

因此有：  

```python
return all(r1[:-1] == r2[1:] for r1, r2 in zip(m, m[1:]))
```

