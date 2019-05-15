[448. Find All Numbers Disappeared in Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)  

### Description

Given an array of integers where 1 ≤ a[i] ≤ *n* (*n* = size of array), some elements appear twice and others appear once.

Find all the elements of [1, *n*] inclusive that do not appear in this array.

Could you do it without extra space and in O(*n*) runtime? You may assume the returned list does not count as extra space.

**Example:**

```
Input:
[4,3,2,7,8,2,3,1]

Output:
[5,6]
```



### Solution

难点在于如何不使用额外空间。

一般做法（使用额外空间）:

```python
class Solution:
    def findDisappearedNumbers(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        table = [0]*(len(nums)+1)
        for num in nums:
            table[num] += 1
        res = []
        for i in range(1,len(nums)+1):
            if table[i] == 0:
                res.append(i)
        return res
```





**评论区大神做法**：  

```python
class Solution:
    def findDisappearedNumbers(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        # For each number i in nums,
        # we mark the number that i points as negative.
        # Then we filter the list, get all the indexes
        # who points to a positive number
        for i in range(len(nums)):
            index = abs(nums[i]) - 1
            nums[index] = -abs(nums[index])
        return [i+1 for i in range(len(nums)) if nums[i] > 0]
```



### 思考

拓展思维面。如何在`nums`本身上做文章！这里的数字其实和`index`有着对应关系，如果某一数字存在，令相应`index`位置的数字取负数。最后遍历一遍即可看看哪些位置的数不是负数的。