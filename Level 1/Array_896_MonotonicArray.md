[896. Monotonic Array](https://leetcode.com/problems/monotonic-array/)  

An array is *monotonic* if it is either monotone increasing or monotone decreasing.

An array `A` is monotone increasing if for all `i <= j`, `A[i] <= A[j]`.  An array `A` is monotone decreasing if for all `i <= j`, `A[i] >= A[j]`.

Return `true` if and only if the given array `A` is monotonic.


**Example 1:**

```
Input: [1,2,2,3]
Output: true

```

**Example 2:**

```
Input: [6,5,4,4]
Output: true

```

**Example 3:**

```
Input: [1,3,2]
Output: false

```

**Example 4:**

```
Input: [1,2,4,5]
Output: true

```

**Example 5:**

```
Input: [1,1,1]
Output: true
```

**Note:**

1. `1 <= A.length <= 50000`
2. `-100000 <= A[i] <= 100000`



### Solution  

**第一次提交**

```python
class Solution(object):
    def isMonotonic(self, A):
        """
        :type A: List[int]
        :rtype: bool
        """
        return all(r1 <= r2 for r1, r2 in zip(A, A[1:])) or all (r1 >= r2 for r1, r2 in zip(A, A[1:]))
```

根据Lee的Python修改的Python3的Code：  

```python
class Solution:
    def isMonotonic(self, A):
        """
        :type A: List[int]
        :rtype: bool
        """
        return not { (i>j)-(i<j) for i, j in zip(A, A[1:])} >= {1, -1}
```



