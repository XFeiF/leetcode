[566. Reshape the Matrix](https://leetcode.com/problems/reshape-the-matrix/)



```python
class Solution(object):
    def matrixReshape(self, nums, r, c):
        """
        :type nums: List[List[int]]
        :type r: int
        :type c: int
        :rtype: List[List[int]]
        """
        nr, nc = len(nums), len(nums[0])
        sz = nr * nc
        if sz != r*c: return nums
        res, cur = [], []
        for i in range(nr):
            for j in range(nc):
                if len(cur) == c:
                    res.append([ele for ele in cur])
                    cur = [nums[i][j]]
                else:
                    cur.append(nums[i][j])
        res.append(cur)
        return res
```



