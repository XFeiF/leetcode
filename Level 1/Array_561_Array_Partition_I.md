[561. Array Partition I](https://leetcode.com/problems/array-partition-i/)  

### 原题

Given an array of **2n** integers, your task is to group these integers into **n** pairs of integer, say (a1, b1), (a2, b2), ..., (an, bn) which makes sum of min(ai, bi) for all i from 1 to n as large as possible.  

### 解释  

把2n个数分成n个2元组，取每个二元组中最小的，使得最终的和最大。思路明确，排序后，每两个数取一个。  

代码也很简洁。

```python
class Solution:
    def arrayPairSum(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        return sum([x for x in sorted(nums)[::2]])
```

