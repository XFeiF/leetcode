[832. Flipping an Image](https://leetcode.com/problems/flipping-an-image/)  

### 原题

Given a binary matrix A, we want to flip the image horizontally, then invert it, and return the resulting image.  

To flip an image horizontally means that each row of the image is reversed.  For example, flipping [1, 1, 0] horizontally results in [0, 1, 1].  

To invert an image means that each 0 is replaced by 1, and each 1 is replaced by 0. For example, inverting [0, 1, 1] results in [1, 0, 0].  

Notes:  

> - 1 <= A.length = A[0].length <= 20  
> - 0 <= A[i][j] <= 1
### 解读 

对二维数组中的每一个一维数组，先左右翻转，再互换0、1值。    

时间复杂度$O(n^2)$

### First Commit

1A beats 44% 的解法：  

```python
class Solution:
    def flipAndInvertImage(self, A):
        """
        :type A: List[List[int]]
        :rtype: List[List[int]]
        """
        res = []
        for item in A:
            item.reverse()
            for idx, x in enumerate(item):
                if x == 1:
                    item[idx] = 0
                else:
                    item[idx] = 1
            res.append(item)
        return res
```

### 如何优化  

1. 异或

   互换0，1值的时候，用了判断，但是因为0和1比较特殊，所以可以用异或来做。但是怎么做呢？第一次尝试过，但是不得其法。想到但不会用。  

   深入想一下，其实可以发现 `0^0 = 0, 1^0 = 1` ，`0^1 = 1, 1^1 = 0`，因此只需要对每个元素都和`1`进行`^`操作即可。  

   这段`for`循环可以优化为: `item = [x^1 for x in item]`，提交试一下，提升了4ms。  

2. reverse

   另外花时间可能比较多的是`reverse`，单独放在了一步，有个单独的开销。

   纵观整个题目，**先左右翻转，再异或**，这两步其实没有先后顺序，**先异或，再翻转**结果是一样的，既然这样，可以一起做吗？

   可以的。由于`python`切片的性质，可以从后往前读元素，例如`item[::-1]`。

补充一点切片的知识：  

> Slice notation "[a: b :c]" means "count in increments of `c` starting at `a` inclusive, up to `b` exclusive". If `c` is negative you count backwards, if omitted it is 1. If `a` is omitted then you start as far as possible in the direction you're counting from (so that's the start if `c` is positive and the end if negative). If `b` is omitted then you end as far as possible in the direction you're counting to (so that's the end if `c` is positive and the start if negative). If `a` or `b` is negative it's an offset from the end (-1 being the last character) instead of the start.

### 优化结果  

```python
class Solution(object):
    def flipAndInvertImage(self, A):
        """
        :type A: List[List[int]]
        :rtype: List[List[int]]
        """
        return [[x^1 for x in row[::-1]] for row in A]
```

