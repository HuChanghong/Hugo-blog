---
title = "Algorithms"
date = 2019-07-01T14:05:47+08:00
draft = true
---


[TOC]
# Algorithms

In this notebook, you will learn the following:

1. Big O Notation
2. Recursion
    - Fibonacci
    - Binary Search
## Array, List, String, Set, Dictionary
List Dictionary 这些都可以看做是一个数据的容器，他们存储数据的方式有不同。
对于数据，我们常用的操作有，插入、删除、查找 等。
不同的存储方式会导致在做这些操作的时候，时间上会有差别。这就是数据结构。

那么什么是算法：
如何评估算法：

算法就是解决问题
* 问题是什么 Problem
* 我们有什么 Input
* 我们想要得到什么 Output
* 尝试最简单的方法 Simple Solution
* 看看如何改进 Develop Incrementally

Algorithm Analysis
| 时间复杂度 | 表示 |
| --- | --- |
| constent | O(1)|
| logarithmic | O(log(n)) |
| linear | O(n) |
| log linear | O(n\*log(n)) |
| quadratic | O(n^2) | 
| cubic | O(n^3) |
| exponential | O(2^n) |


## Big O Notation
大O表示法
>把我们运行的时间 running time 与 n 的增长关系用一个曲线画出来，如果存在 这样一个函数 k\*f(n)的曲线 当 n >= n0 时，有 k\*f(n) >  running time ，我们就说这个算法的运行时间是 Big O of f(n) 。

### $O(1)$
```python
def square(x):
    return x * x
```

### $O(n)$
```python
def find_max(l):
    import time
    start = time.time()
    if l == None:
        return None
    mx = l[0]
    for n in l:
        if n > mx:
            mx = n
            
    t = time.time() - start
    
    return mx, len(l), t
```

### $O(n^2)$




### Time Complexity & Space Complexity


时间复杂度&空间复杂度



## Searching and Sorting

比如：
**x > 1 , 求x的平方根y, 0 < y < x**
1. 设Low为0，High为x
2. 假设Guess是(Low+High)/2，如果Guess的平方非常接近x，那么y=g
3. 若，g\*g < x，L设定为Guess，然后重复第二步
4. 否则，g\*g > x，H设定为Guess，然后重复第二步

>按步骤，告诉计算机解决问题的方法

Q:比如最常见的排序，如何排序：
A:简单，用.sort()  用 sorted()
Q:那么时间复杂度是多少呢： 
A:我知道，python里 sort 是O(n\*log(n))
Q:那么为什么是O(n\*log(n))呢？
A:额。。。

好吧我们下面来学习这些内容：
* Sort
* Binary Search
* Divide and Conquer
* Two Pointers
* Sliding Window
* Others
* Greedy
* Dynamic Programming \*


### Bubble Sort

### Insertion Sort

### Merge Sort

### Quick Sort





## Divide and Conquer
分治法

- **Divide and Conqure (*Recursion*)**
>Recursion in computer science is a method where the solution to a problem depends on solutions to smaller instances of the same problem (as opposed to iteration). The approach can be applied to many types of problems, and recursion is one of the central ideas of computer science.

### Fibonacci
Fibonacci 的递归算法：
```python
def fibonacci(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fibonacci(n-1) + fibonacci(n-2)
```
就像数学思想中的 数学归纳法：
首先有一个 `Base` ，作为一个起点， 然后不断递推。

>那么上述算法的时间复杂度是多少？

O(2^n)

怎么理解：
当我们需要计算一个f(10)的时候，我们会计算一个f(9)和f(8)，然后f(9)需要计算一个f(8)和f(7)。注意这里，f(8)已经需要被计算2次，以此类推，f(7)需要被计算4次...f(1)需要被计算 2^(10-1) 次，f(0)需要被计算2^10

>时间复杂度更低的实现：不用递归
```python
def fibonacci2(n):
    assert(n>=0)
    a, b = 0, 1
    for i in range(1, n+1):
        a, b = b, a+b
    return a
```
时间复杂度为：
O(n)

>如果用递归，如何优化时间复杂度：

思路： 把算过的结果存起来

```python
def fibonacci3(n):
    assert(n>=0)
    if(n <= 1):
        return(n, 0)
    (a, b) = fibonacci3(n-1)
    return (a+b, a)
```

>但是递归算法在Python不同的版本中都会有次数的上限：为了防止无穷无尽的开辟新的内存空间。

RecursionError： 递归次数超过上限。 （2960次，Python版本：3.7.3）

### Binary Search

Question: Given a sorted list of numbers, find the index of a specific value in the list. If no such value, return -1.

**Solution 1**: Brute Force
暴力枚举：是否有序都可以用
时间复杂度 O(n)
```python
def search(num_list, val):
    # If empty
    if num_list == None:
        return -1
    
    for i in range(0, len(num_list)):
        if (num_list[i] == val):
            return i
    return -1
```

**Solution 2**: Binary Search
因为是一个有序的list，所以可以通过对比大小进行二分查找：
1. list取大约中间的下标mid = (low + high)/2，用value跟需要找的值比较，相等则返回mid
2. 若value小，缩减下一轮比较list下标范围，low= mid+1
3. 否则，缩减下一轮比较list下标范围，high= mid-1
4. 终止条件：当下标缩减至l>h，说明全找完了，没找到，返回-1


同样的逻辑可以有以下两种写法：

```python
# Binary Search (recursive)
def bi_serach_re(num_list, val):
    def bi_search(l, h):
        # Not found
        if l > h:
            return -1
        
        # Check mid
        mid = (l + h) // 2
        if (num_list[mid] == val):
            return mid;
        elif (num_list[mid] < val):
            return bi_search(mid + 1, h)
        else:
            return bi_search(l, mid - 1)
        
    return bi_search(0, len(num_list))
```


```python
# Binary Search (iterative)
def bi_search_iter(num_list, val):
    l = 0
    h = len(num_list)
    while (l <= h):
        mid = (l + h) // 2
        if (num_list[mid] == val):
            return mid
        elif (num_list[mid] < val):
            l = mid + 1
        else:
            h = mid - 1
    return -1
```






