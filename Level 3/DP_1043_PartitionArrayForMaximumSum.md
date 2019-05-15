[1043. Partition Array for Maximum Sum](https://leetcode.com/problems/partition-array-for-maximum-sum/)  

### Description

Given an integer array A, you partition the array into (contiguous) subarrays of length at most K.  After partitioning, each subarray has their values changed to become the maximum value of that subarray.

Return the largest sum of the given array after partitioning.

**Example:**

```
Input: A = [1,15,7,9,2,5,10], K = 3
Output: 84
Explanation: A becomes [15,15,15,9,10,10,10]
```

### Solution
**注意：** 题中的`K`表示子串的最大长度，而不是分成的子串的组数。  

首先可以判断出这是一道DP问题。然后我们再去想状态以及状态转移方程。  

从样例出发，考虑`K=1, 2, 3`的情况。  

当`K=1`的时候，因为子串只能是单个元素，所以结果就是数组所有元素的和。

当`K=2`的时候，我们从前向后逐个元素考虑。  
```
A[0], 只有一个元素，所以此时最大和为1；  
A[0,1]，有两个元素，且最大子串长度为2，故最大和是15*2 = 30；
A[0,1,2]，这个时候有三个元素，有两种情况（这里忽略每组只有1个元素的情况）：  
    1. A[0,1] + A[2] -> 30 + 7 = 37
    2. A[0] + A[1, 2] -> 1 + 30 = 31
A[0,1,2,3]，同上面的情况分析：
    1. A[0,1,2] + A[3] -> 37 + 9 = 46  
    # 注意这里利用了我们上一步已经得出的信息A[0, 1, 2]最好的结果
    2. A[0,1] + A[2,3] -> 30 + 18 = 48
    3. A[0] + A[1, 2, 3]               
    # 这里我们是不是要考虑一下，这一步做不做？因为这和第一种情况其实就是反过来的
    # 而且这里我们不知道A[1,2,3]的结果。换句话，这不是已知的子问题了。  
# 到这一步我们大致可以得出，这里的状态dp[i]应该是A[0...i-1]的最大子串和。
# 另外我们需要思考一个问题，当向后扩展一个新的元素的时候，我们向前比较多少次？
# 我们继续向下再扩展一个元素  
A[0,1,2,3,4]： # 我们总是先考虑最好考虑的情况
    1. A[0,1,2,3] + A[4] -> 48 + 2 = 50
    2. A[0,1,2] + A[3,4] -> 37 + 18 = 55
# 对比上一步，这里的发现是，每一步中相加的两个元素有这样的规律：前者是我们之前算过
# 的已知结果，后者的始终长度从1变化到2，即不超过K。
# 就是说，加上这个新的元素后，每次回退不超过K步。  
我们在K=3里验证这个想法。  
```
当`K=3`时，有：
```
A           dp[0] = 0
A[0]        dp[1] = 1
A[0,1]      dp[2] = 30
A[0,1,2]    dp[3] = 45 
# A[0,1] + A[2] = 37
# A[0] + A[1,2] = 31
# [] + A[0,1,2] = max(A[0,1,2])*3 = 45  
A[0,1,2,3] dp[4] = 54
# A[0,1,2] + A[3]   45+9=54
# A[0,1] + A[2,3]   30+18=48
# A[0] + A[1,2,3]   1+45=46
...
```  

至此，得出：  用`dp[i]` 记录我们可以得到的`A[0...i-1]`的最大和。
为了得到`dp[i+1]`，我们需要分别group最后k个元素，取最大者。

复杂度：
Time O(NK), Space O(N)

### Python    
#### v1
```python
class Solution:
    def maxSumAfterPartitioning(self, A: List[int], K: int) -> int:
        if not A: return 0
        if K == 1: return sum(A)
        dp = [0]*(len(A)+1)
        for i in range(1, len(A)+1):
            cur = dp[i-1] + A[i-1]
            for j in range(1, K+1):
                if i - j >= 0:
                    tmp = dp[i-j] + max(A[(i-j):i]) * j
                    cur = max(tmp, cur)
            dp[i] = cur
        return dp[-1]
```  
这份代码的问题在于每次都需要求`A[(i-j):i]`中的最大者，很浪费时间，所以最终耗时只击败7%的人。

#### v2  
这一版参考了lee的代码，因为`v1`中最开始定义的`cur`其实就是初始化的作用。我们可以用它来做记忆，记忆group最后k个元素的最大元素，无非就是从最后一个到最后第k个，而且我们也是这样遍历过来的。  
```python  
class Solution:
    def maxSumAfterPartitioning(self, A: List[int], K: int) -> int:
        if not A: return 0
        if K == 1: return sum(A)
        dp = [0]*(len(A)+1)
        for i in range(1, len(A)+1):
            curMax = 0
            for j in range(1, K+1):
                if i - j >= 0:
                    curMax = max(curMax, A[i-j])
                    dp[i] = max(dp[i], dp[i-j] + curMax * j)
        return dp[-1]
```  

#### v3  
这一份还是没有达到lee的水平，原因出在第二个`for`循环里，需要`if`条件，这一步完全可以放到`for`中限制遍历次数，因此有了最终版：  
```python
class Solution:
    def maxSumAfterPartitioning(self, A: List[int], K: int) -> int:
        if not A: return 0
        if K == 1: return sum(A)
        dp = [0]*(len(A)+1)
        for i in range(1, len(A)+1):
            curMax = 0
            for j in range(1, min(K, i)+1):
                curMax = max(curMax, A[i-j])
                dp[i] = max(dp[i], dp[i-j] + curMax * j)
        return dp[-1]
```
这一版在时间上可以快于86.63%的提交，Memory Usage 13.1MB。