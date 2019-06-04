[215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)    

简单题，使用堆。  
Python 1pass code：  
```python  
import heapq
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        heapq.heapify(nums)
        # return heapq.nlargest(k, nums)[-1]
        for i in range(k):
            if i == k - 1:
                return heapq._heappop_max(nums)
            else:
                heapq._heappop_max(nums)
```  

## 思考  
思路很清晰，解法也很多。  
重要的还是掌握堆结构的性质与实现。  