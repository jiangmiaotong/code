**剑指offer——牛客网**  
=
***
003 重建二叉树
-
```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回构造的TreeNode根节点
    def reConstructBinaryTree(self, pre, tin):
        # write code here
        if pre == []:     
            return None
        root = TreeNode(pre[0])
        cut = tin.index(pre[0])
        root.left = self.reConstructBinaryTree(pre[1:cut+1], tin[:cut])
        root.right = self.reConstructBinaryTree(pre[cut+1:], tin[cut+1:])
        return root
```

```python
class Solution:
    # 返回构造的TreeNode根节点
    def reConstructBinaryTree(self, pre, tin):
        # write code here
        if pre == []:
            return None
        for i in range(len(pre)):
            if tin[i] == pre[0]:
                break
        root = TreeNode(pre[0])
        #cut = tin.index(pre[0])
        root.left = self.reConstructBinaryTree(pre[1:i+1], tin[:i])
        root.right = self.reConstructBinaryTree(pre[i+1:], tin[i+1:])
        return root
```
**NOTE: None 不等同于空list**  
**解题关键：熟悉二叉树遍历定义，前序遍历最先访问根结点，中序遍历访问完左子树，再访问根结点。** 

004 用两个栈实现队列
-
```python
-*- coding:utf-8 -*-
class Solution:
    def __init__(self):
        self.stack1 = []
        self.stack2 = []
    def push(self, node):
        # write code here
        self.stack1.append(node)
    def pop(self):
        # return xx
        if self.stack2 != []:
            return self.stack2.pop()
        if self.stack1 == []:
            return None
        while self.stack1 != []:
            self.stack2.append(self.stack1.pop())
        return self.stack2.pop()  #pop()自带对空list的处理
```
**解题思路：栈和队列的区别只在pop的时候有所体现，因此对删除元素的几种情形进行画图举例，即可得出解题思路。**  

005 旋转数组的最小数字
-
```python
# -*- coding:utf-8 -*-
class Solution:
    def minNumberInRotateArray(self, rarray):
        # write code here
        if rarray == []:
            return 0
        l = 0
        r = len(rarray) - 1
        if rarray[l] < rarray[r]:
            return rarray[0]
        while (r-l) > 1:
            mid = (l+r)/2
            if (rarray[l] == rarray[r]) & (rarray[mid] == rarray[l]): #此时无法判断最小元素可能出现在哪侧，因此无法缩小搜索范围
                return self.minarray(rarray[l,r])  #顺序查找 O(n)
            if rarray[mid] >= rarray[l]:
                l = mid
            else:
                r = mid
        return rarray[r]
    def minarray(self, array):
        m = array[0]
        for i in array:
            if i < m:
                m = i
        return m
```
**解题关键：首先分析旋转数组头尾数字（<, =, >）的三种情况，同时类排序数组联想到二分查找（头尾两根指针），再举例分析查找过程中mid对应数字相对于数组头尾元素的三种情况，统计规律，形成思路。**    

006 斐波那契数列
-
```python
递归写法：简单但重复计算量大，超时。
# -*- coding:utf-8 -*-
class Solution:
    def Fibonacci(self, n):
        # write code here
        if n == 0:
            return 0
        if n == 1:
            return 1
        return self.Fibonacci(n-1) + self.Fibonacci(n-2)
非递归写法：将重复计算以中间量暂存下来，节约计算量——迭代
# -*- coding:utf-8 -*-
# -*- coding:utf-8 -*-
class Solution:
    def Fibonacci(self, n):
        # write code here
        if n == 0:
            return 0
        if n == 1:
            return 1
        f1 = 0
        f2 = 1
        for _ in range(n-1):
            f1, f2 = f2, f1+f2
        return f2
```
**知识点：菲波那切数列 f(n) = f(n-1) + f(n-2)**  

007 青蛙跳台阶
-
```python
# -*- coding:utf-8 -*-
class Solution:
    def jumpFloor(self, number):
        # write code here
        f1 = 1
        f2 = 1
        while (number > 1):
            f1, f2 = f2, f1+f2
            number = number - 1 
        return f2
```
**解题思路：从一级台阶开始归纳，一级只能先跳1，对应方法有f(0)=1种，2级可以先跳1，对应方法有f(2-1)种，也可以先跳2，对应方法有f(2-2)=f(0)=1种，，，f(n) = f(n-1) + f(n-2)**  

008 变态跳台阶
-
```python
# -*- coding:utf-8 -*-
class Solution:
    def jumpFloorII(self, number):
        # write code here
        l = [1]
        while number > 1 :
            l.append(sum(l))
            number = number - 1
        return sum(l)
```
**解题思路：从一级台阶开始归纳，f(n) = f(n-1) + f(n-2) + ... + f(0),需要保存所有中间结果。**  

009 矩形覆盖
-
```python
# -*- coding:utf-8 -*-
class Solution:
    def rectCover(self, number):
        # write code here
        if number == 0:
            return 0
        f1 = 1 
        f2 = 1
        while number > 1:
            f1, f2 = f2, f1+f2
            number = number - 1
        return f2
```
**解题思路：可以类比于跨台阶，横放小矩形时必须一次放两个，就相当于一步跨两个。因此还是斐波那契数列。**  

010 二进制中1的个数
-

```python
**错误解法：采用flag左移的方式时，由于python中的int是无限大的，会发生左移进入死循环的情况。
c++使用unsigned int flag进行循环限制。**
# -*- coding:utf-8 -*-
class Solution:
    def NumberOf1(self, n):
        # write code here
        count = 0
        flag = 1
        while(flag):
            if (n & flag):
                count = count + 1 
            flag = flag << 1
        return count
```
```python
# -*- coding:utf-8 -*-
**手动进行位数限制**
class Solution:
    def NumberOf1(self, n):
        # write code here
        count = 0
        flag = 1
        for _ in range(32):
            if (n & flag):
                count = count + 1 
            flag = flag << 1
        return count
```

```python
**右移解法:负数用补码表示。**
# -*- coding:utf-8 -*-
class Solution:
    def NumberOf1(self, n):
        # write code here
        count = 0
        if n < 0:
            n = n & 0xffffffff
        while(n):
            if (n & 1):
                count = count + 1
            n = n >> 1
        return count
```
```python
**将1逐个消除的解法，需要循环次数最少**
# -*- coding:utf-8 -*-
class Solution:
    def NumberOf1(self, n):
        # write code here
        bits = 0
        if n < 0:
            n = n & 0xffffffff   
#python中负数使用-ob加原码表示，与0xffffffff（0b11111111111111111111111111111111）相与，不改变n的情况下，将n转成补码表示。
        while n:
            bits += 1
            n = (n-1) & n
        return bits
```
**知识点：位运算会把数字用二进制表示（并不会将操作符左右两边数字补成相同位数），可对每一位上的0或者1做与(&)、或(|)、异或(^)、左移(<<)、右移(>>)操作。需要注意的是左移n位操作直接丢弃最左边n位，最右边补上n个0；右移n位操作丢弃最右边n位，但是数字为正时最左边补n位0，数字为负时最左边补n位1。因此，需要特别注意位运算中负数情况的处理。**  

00
-
```python

```
****  

00
-
```python

```
****  

00
-
```python

```
****  

00
-
```python

```
****  

00
-
```python

```
****  

00
-
```python

```
****  
