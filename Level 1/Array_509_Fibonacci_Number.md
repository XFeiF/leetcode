[509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)  

直接打表。

```python
class Solution(object):
    def fib(self, N):
        """
        :type N: int
        :rtype: int
        """
        # F = [0, 1]
        # for i in range(2, 31):
        #     cur = F[i-1] + F[i-2]
        #     F.append(cur)
        # print(F)
        F = [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 1597, 2584, 4181, 6765, 10946, 17711, 28657, 46368, 75025, 121393, 196418, 317811, 514229, 832040, 1346269]
        return F[N]
        
```

