### [1106. Parsing A Boolean Expression](https://leetcode.com/problems/parsing-a-boolean-expression/)  

总结：直觉的数学思路（化简） + 一些 python 语言细节。

1WA，2TLE  以及参考了其他人的做法才解出来。    

首先简单的地方在于最简单的基本情况只有一个字符，很好判断。  

复杂的地方在于表达式嵌套，想找到一种方法解嵌套。   

自己举几个例子可以发现，每一个基本情况都需要计算。也就是与或非三个基本表达式，那么问题转化为将每个基本情况都计算出来，然后进行化简。  

再深入一层，需要找到每个基本情况。

我最开始尝试采用递归的方法，肯定可以做出来。但是我没有，我在处理一些特殊情况的时候，考虑不全面以及写的很复杂，相当于是每种特殊情况我都要列出来……  

打开讨论界面，标题里出现次数比较多的关键词是`stack`。  

考虑一下，用栈的方式的话，读取表达式，每次都可以先处理一个基本情况，将基本情况转化为`t`或者`f`，这样一直把复杂表达式中子表达式化简成最简形式，最后得到的表达式也就是最简形式了。

思路大概是这样，python的代码如下：

```python
class Solution:
    def parseBoolExpr(self, exp: str) -> bool:
        n = len(exp)
        if n == 0: return False
        
        exp_stack = []  # 保存 操作符，括号以及最基本单元 t, f
        op_stack = []  # 保存 操作符在表达式中的位置 （间接）
        
        for idx in range(n):
            if exp[idx] == ',':  # 跳过
                continue
            if exp[idx] != ')':  # 表示还不是一个子表达式的结束
                exp_stack.append(exp[idx])
                if exp[idx] == '(':  # 一个子表达式的开始
                    op_stack.append(idx)
            else:
                op = exp[op_stack.pop() - 1]  # 取出当前最小表达式的操作符
                cur = exp_stack.pop() == 't'  # 针对 “!” 的特殊处理，因为其基础表达式内只包含一个字符
                while exp_stack[-1] != '(':   # 一直弹出栈直到该最小表达式结束
                    if op == '&':
                        cur = (exp_stack.pop() == 't') and cur  # 注意python and or的特性，只能放前面
                    elif op == '|':
                        cur = (exp_stack.pop() == 't') or cur  # 待计算的表达式只能放前面
                exp_stack.pop()  # 弹出"("
                exp_stack.pop()  # 弹出操作符
                if op == '!': cur = not cur
                res = 't' if cur else 'f'  # 将子表达式转化为最简单的基本情况
                exp_stack.append(res)
                
        return exp_stack.pop() == 't'
        
```



